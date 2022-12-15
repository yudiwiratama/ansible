---
title: "Cara Menyiapkan High Availability Kubernetes Klaster Menggunakan Kubeadm"
description: "Cara menyiapkan klaster kubernetes yang High Available"
date: "2021-09-10"
tags: ["Kubernetes", "HA"]
categories: ["Kubernetes"]
ShowToc: true
TocOpen: true
---

# Topologi
![](/images/kubeadm-ha-topology-stacked-etcd.svg)

# Versi
## Sistem Operasi
Ubuntu 20.04 (Focal)

## Aplikasi
- HAProxy 2.0.13-2ubuntu0.3
- KeepAlived 1:2.0.19-2
- Containerd 1.4.9-1
- Kubernetes 1.22.1-00 (kubeadm, kubelet, kubectl)

# Panduan
- Semua perintah dibawah dijalankan menggunakan user `root`

## Persiapan
1. Sunting file `/etc/hosts`
```bash
cat<<EOF >> /etc/hosts

### Lab Kubernetes ###
192.168.30.10 kubernetes-vip
192.168.30.11 kubernetes-lb1
192.168.30.12 kubernetes-lb2
192.168.30.13 kubernetes-master01
192.168.30.14 kubernetes-master02
192.168.30.15 kubernetes-master03
192.168.30.16 kubernetes-worker01
EOF
```

2. `update` dan `upgrade` package-package  
```bash
apt update -y; apt upgrade -y
```

## Pasang HAproxy & Keepalived pada Load Balancer Node
1. Pasang haproxy dan keepalived 
```bash
apt install -y haproxy keepalived
```

2. Konfigurasi HAProxy
    1. Sunting file konfigurasi
    - Tambahkan baris berikut kedalam file konfigurasi. Isikan _backend_ dengan Alamat IP semua master
    ```bash
    vim /etc/haproxy/haproxy.cfg
    ```
    ```
    frontend kube-apiserver
        bind *:6443
        mode tcp
        option tcplog
        default_backend kube-apiserver
    
    backend kube-apiserver
        mode tcp
        option tcp-check
        balance roundrobin
        default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100
        server kube-apiserver-1 192.168.30.13:6443 check
        server kube-apiserver-2 192.168.30.14:6443 check
        server kube-apiserver-3 192.168.30.15:6443 check
    ```

    2. Validasi file konfigurasi
    ```bash
    haproxy -c -V -f /etc/haproxy/haproxy.cfg
    ```

    3. Restart HAProxy
    ```bash
    systemctl restart haproxy
    ```

    4. Enable HAProxy
    ```bash
    systemctl enable haproxy
    ```

3. Konfigurasi Keepalived
    1. Sunting file keepalived
    ```bash
    vim /etc/keepalived/keepalived.conf
    ```

    - lb01
    ```
    vrrp_script chk_haproxy {
      script "killall -0 haproxy"
      interval 2
      weight 2
    }
    
    vrrp_instance haproxy-vip {
      state MASTER
      priority 100
      interface ens3                      # Network card
      virtual_router_id 50
      advert_int 1
      authentication {
        auth_type PASS
        auth_pass nasgorenak
      }
    
      virtual_ipaddress {
        192.168.30.10/24                  # The VIP address
      }
    
      track_script {
        chk_haproxy
      }
    }
    ```

    - lb02
    ```
    vrrp_script chk_haproxy {
      script "killall -0 haproxy"
      interval 2
      weight 2
    }
    
    vrrp_instance haproxy-vip {
      state BACKUP
      priority 99
      interface ens3                      # Network card
      virtual_router_id 50
      advert_int 1
      authentication {
        auth_type PASS
        auth_pass nasgorenak
      }
    
      virtual_ipaddress {
        192.168.30.10/24                  # The VIP address
      }
    
      track_script {
        chk_haproxy
      }
    }
    ```

    2. Restart Keepalived
    ```bash
    systemctl restart keepalived
    ```

    3. Enable Keepalived
    ```bash
    systemctl enable keepalived
    ```

## Pasang container runtime pada K8S master & node
Kita akan menggunakan containerd
1. Muat kernel module dan ubah beberapa parameter kernel
```bash
cat <<EOF | tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

# Setup required sysctl params, these persist across reboots.
cat <<EOF | tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# Apply sysctl params without reboot
sysctl --system
```

2. Pasang repository resmi docker
```bash
# Pasang GPG key repository docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Tambahkan repository docker
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

apt update
```

3. Pasang containerd.io
```bash
apt install containerd.io
```

4. Konfigurasi containerd
```bash
sudo mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
```

5. Restart containerd
```bash
systemctl restart containerd
```

## Pasang package-package kubernetes pada master & worker
1. Pasang repository
```bash
# Pasang GPG key repository kubernetes
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Tambahkan repository kubernetes
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list

apt update
```

2. Pasang package-package kubernetes
```bash
apt install -y kubelet=1.22.1-00 kubeadm=1.22.1-00 kubectl=1.22.1-00
apt-mark hold kubelet kubeadm kubectl
```

> Tips!  
> Periksa versi package yang tersedia menggunakan `apt-cache madison <name_package>`

## Bootstrap Master (kubernetes-master01)
1. Bootstrap klaster kubernetes
```bash
kubeadm init --pod-network-cidr=172.16.0.0/16 --control-plane-endpoint "kubernetes-vip:6443" --upload-certs
```

![](/images/ha-kubernetes-kubeadm-1.png)

  - Output kubeadm init  
  Simpan dan gunakan untuk mendaftarkan node lain ke dalam klaster
  ```bash
  To start using your cluster, you need to run the following as a regular user:
  
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
  
  Alternatively, if you are the root user, you can run:
  
    export KUBECONFIG=/etc/kubernetes/admin.conf
  
  You should now deploy a pod network to the cluster.
  Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
    https://kubernetes.io/docs/concepts/cluster-administration/addons/
  
  You can now join any number of the control-plane node running the following command on each as root:
  # Gunakan ini untuk mendaftarkan master
    kubeadm join kubernetes-vip:6443 --token tzkfd1.tsgtqfy5q15weour \
          --discovery-token-ca-cert-hash sha256:e4c1619f9050ac1041e198675a7bf1d275a1d8a486ba213260d0cd4144a46b89 \
          --control-plane --certificate-key 496abde789dc5340843bf83272bb3f307ae13bd6f4ad2fc3e9dfe08cc77aa991
  
  Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
  As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
  "kubeadm init phase upload-certs --upload-certs" to reload certs afterward.
  
  Then you can join any number of worker nodes by running the following on each as root:
  # Gunakan ini untuk mendaftarkan worker
  kubeadm join kubernetes-vip:6443 --token tzkfd1.tsgtqfy5q15weour \
          --discovery-token-ca-cert-hash sha256:e4c1619f9050ac1041e198675a7bf1d275a1d8a486ba213260d0cd4144a46b89
  ```

2. Pindahkan kubeconfig  
**kubeconfig** berisikan informasi mengenai endpoint kubernetes dan token untuk autentikasi
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

3. Pasang CNI

    1. Unduh manifest calico
    ```bash
    curl https://docs.projectcalico.org/manifests/calico.yaml -O
    ```

    2. Sunting calico.yaml
    ```bash
    # Sunting calico.yaml
    vim calico.yaml
    ```
    
    ```
    # sesuaikan CALICO_IP4POOL_CIDR dengan --pod-network-cidr pada saat bootstrap
                - name: CALICO_IPV4POOL_CIDR
                  value: "172.16.0.0/16"
    ```

    3. Terapkan pada klaster
    ```bash
    kubectl apply -f calico.yaml
    ```

## Mendaftarkan Master ke Klaster
1. Daftarkan master

Perintah dibawah didapatkan dari output kubeadm init pada master01
```bash
kubeadm join kubernetes-vip:6443 --token tzkfd1.tsgtqfy5q15weour \
      --discovery-token-ca-cert-hash sha256:e4c1619f9050ac1041e198675a7bf1d275a1d8a486ba213260d0cd4144a46b89 \
      --control-plane --certificate-key 496abde789dc5340843bf83272bb3f307ae13bd6f4ad2fc3e9dfe08cc77aa991
```
![](/images/ha-kubernetes-kubeadm-2.png)

## Mendaftarkan Worker ke Klaster

Perintah dibawah didapatkan dari output kubeadm init pada master01
1. Daftarkan worker
```bash
kubeadm join kubernetes-vip:6443 --token tzkfd1.tsgtqfy5q15weour \
        --discovery-token-ca-cert-hash sha256:e4c1619f9050ac1041e198675a7bf1d275a1d8a486ba213260d0cd4144a46b89
```
![](/images/ha-kubernetes-kubeadm-3.png)

# Hasil
![](/images/ha-kubernetes-kubeadm-4.png)

# Uji coba High Availability
1. Matikan salah satu lb
```bash
systemctl poweroff
```
![](/images/ha-kubernetes-kubeadm-5.png)

2. Periksa status klaster dengan menjalankan perintah kubectl
```bash
# Sebagai contoh
kubectl get nodes
```
![](/images/ha-kubernetes-kubeadm-6.png)

# Referensi
- [Kubesphere - HA Cluster Using Keepalived & Haproxy](https://kubesphere.io/docs/installing-on-linux/high-availability-configurations/set-up-ha-cluster-using-keepalived-haproxy/)
- [Docker Docs - Install Repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
- [Kubernetes Docs - Container Runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
- [Kubernetes Docs - Install kubeadm, kubelet, kubectl](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [Kubernetes Docs - kubeadm HA](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/)