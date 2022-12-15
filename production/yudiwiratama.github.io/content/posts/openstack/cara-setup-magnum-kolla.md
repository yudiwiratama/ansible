---
title: "Cara Setup OpenStack Magnum Dengan Deployment Kolla-Ansible [Kubernetes COE]"
description: "Cara mudah setup openstack magnum dengan deployment kolla-ansible"
date: "2021-08-30"
tags: ["OpenStack", "Magnum", "Orchestration"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- **Sistem Operasi:** CentOS 8
- **OpenStack:** Ussuri
- **Kolla-Ansible:** 10.3.0
- **Kubernetes Image:** Fedora CoreOS 32

# Panduan
1. Install Dependensi
```bash
sudo dnf install -y python3-devel libffi-devel gcc openssl-devel python3-libselinux
```

2. Siapkan python3 virtual environment
```bash
python3 -m venv kolla
source kolla/bin/activate
```

3. Upgrade package pip
```bash
pip install -U pip
```

4. Install kolla-ansible & ansible
```bash
pip install 'ansible<2.10' kolla-ansible==10.3.0
```

5. Buat direktori /etc/kolla
```bash
sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla
```

6. Salin globals.yml dan password.yml ke direktori /etc/kolla
```bash
cp -r kolla/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
```

7. Salin all-in-one file inventory ke direktori sekarang
```bash
cp kolla/share/kolla-ansible/ansible/inventory/multinode .
```

8. Konfigurasi ansible
```bash
sudo mkdir /etc/ansible
sudo bash -c 'cat<<EOF > /etc/ansible/ansible.cfg
[defaults]
host_key_checking=False
pipelining=True
forks=100
EOF'
```

9. _Generate_ password openstack
```bash
kolla-genpwd
```

10. Sunting globals.yml
```bash
cat<<EOF > /etc/kolla/globals.yml
kolla_base_distro: "centos"
kolla_install_type: "binary"
openstack_release: "ussuri"
kolla_internal_vip_address: "10.10.100.6"
kolla_external_vip_address: "10.10.100.7"
network_interface: "eth0"
neutron_external_interface: "eth1"
enable_openstack_core: "yes"
enable_cinder: "yes"
enable_cinder_backup: "no"
enable_magnum: "yes"
enable_cluster_user_trust: true
enable_cinder_backend_lvm: "yes"
enable_neutron_provider_networks: "yes"
nova_compute_virt_type: "kvm"
EOF
```

11. Buat LVM untuk OpenStack Cinder
```bash
sudo dnf install -y lvm2
sudo pvcreate /dev/vdb
sudo vgcreate cinder-volumes /dev/vdb
```

12. Deploy OpenStack Kolla
```bash
sudo dnf install -y epel-release
sudo dnf install -y screen
screen -R kolla
kolla-ansible -i all-in-one bootstrap-servers
kolla-ansible -i all-in-one prechecks
kolla-ansible -i all-in-one deploy
kolla-ansible -i all-in-one post-deploy
```

13. Install openstack client
```bash
source kolla/bin/activate
pip install python-openstackclient python-magnumclient python-heatclient
```

14. Buat flavor untuk instance magnum
```bash
source /etc/kolla/admin-openrc.sh
sudo chown $USER:$USER /etc/kolla/admin-openrc.sh
openstack flavor create m0-kubernetes --disk 20 --vcpu 4 --ram 4096 --public
```

15. Unduh dan ekstrak image Fedora CoreOS 32
```bash
sudo dnf install -y wget
wget https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/32.20200629.3.0/x86_64/fedora-coreos-32.20200629.3.0-openstack.x86_64.qcow2.xz
unxz fedora-coreos-32.20200629.3.0-openstack.x86_64.qcow2.xz
```

16. Unggah image ke OpenStack Glance
```bash
openstack image create \
  --file fedora-coreos-32.20200629.3.0-openstack.x86_64.qcow2 \
  --disk-format qcow2 \
  --container-format=bare \
  --property os_distro=fedora-coreos \
  --property os_admin_user=core \
  --public \
  Fedora-CoreOS-32
```

17. Buat _template_ klaster 
```bash
openstack coe cluster template create k8s-btech-bicara \
    --image Fedora-CoreOS-32 \
    --external-network public1 \
    --dns-nameserver 8.8.8.8 \
    --flavor m0-kubernetes \
    --master-flavor m0-kubernetes \
    --docker-volume-size 5 \
    --network-driver flannel \
    --coe kubernetes \
    --volume-driver cinder
```

18. Buat klaster Kubernetes
```bash
openstack coe cluster create k8s-cluster-btech-bicara --keypair mykey \
  --cluster-template k8s-btech-bicara \
  --labels keystone_auth_enabled=true,kube_tag=v1.18.6,cloud_provider_enabled=true,cinder_csi_enabled=true,cinder_csi_plugin_tag=v1.18.0
```