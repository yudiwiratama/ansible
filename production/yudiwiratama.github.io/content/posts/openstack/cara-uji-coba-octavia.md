---
title: "Cara Uji Coba OpenStack Octavia (CLI)"
description: "Cara uji coba OpenStack Octavia (CLI)"
date: "2022-02-15"
tags: ["OpenStack", "Octavia", "Load Balancer"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- OpenStack Core Service
- Openstack Octavia
- python-octaviaclient (CLI Client)
```bash
pip install python-octaviaclient
```

# Panduan
1. Buat Instance
```bash
# Buat cloud-config file
cat<<EOF > user-data.yaml
#cloud-config
chpasswd:
  list: |
     ubuntu:rahasia
  expire: false
runcmd:
- echo \$(hostname) > index.html
- python3 -m http.server 80
EOF

# Buat instance
openstack server create \
  --user-data user-data.yaml \
  --image ubuntu-focal \
  --flavor m1.small \
  --boot-from-volume 5 \
  --network intnet \
  --key-name deployer \
  --min 3 \
  --max 3 \
  web-basic
```

2. Buat Load Balancer
```bash
openstack loadbalancer create --name testlb --vip-network-id extnet

# Tunggu sampai status menunjukkan status ACTIVE
openstack loadbalancer show testlb
```

3. Buat load balancer listener

Listener adalah port yang akan digunakan oleh load balancer sebagai port untuk melayani `request` atau yang biasa disebut dengan `frontend` pada konfigurasi HAProxy
```bash
openstack loadbalancer listener create --name testlistener --protocol HTTP --protocol-port 80 testlb
```

4. Buat pool

pool berisikan informasi tentang algoritma, listener, dan protokol apa yang ingin digunakan 
```bash
openstack loadbalancer pool create --name testpool --lb-algorithm ROUND_ROBIN --listener testlistener --protocol HTTP
```

5. Buat health monitor
Health monitor digunakan untuk memantau status anggota pool

```bash
openstack loadbalancer healthmonitor create --delay 5 --max-retries 4 --timeout 10 --type HTTP --url-path / testpool
```

6. Buat member pada load balancer

member berisikan layanan yang akan di-*load balance* atau yang biasa disebut dengan `backend` pada konfigurasi HAProxy
```bash
openstack loadbalancer member create --subnet-id intnet-subnet --address $WEB_BASIC_1_ADDR --protocol-port 80 testpool
openstack loadbalancer member create --subnet-id intnet-subnet --address $WEB_BASIC_2_ADDR --protocol-port 80 testpool
openstack loadbalancer member create --subnet-id intnet-subnet --address $WEB_BASIC_3_ADDR --protocol-port 80 testpool

# Contoh:
# openstack loadbalancer member create --subnet-id intnet-subnet --address 192.168.10.10 --protocol-port 80 testpool
```

# Contoh
1. Menggunakan `for`
```bash
for i in {1..10}; do curl IP_ADDRESS; done
```
![](/images/cara-uji-coba-openstack-octavia-cli.png)

2. Menggunakan `while`. Tekan Ctrl + C untuk menghentikan
```bash
while sleep 1; do curl IP_ADDRESS; done
```
![](/images/cara-uji-coba-openstack-octavia-cli-2.png)

# Referensi
- [Octavia Basic CookBook](https://docs.openstack.org/octavia/latest/user/guides/basic-cookbook.html)