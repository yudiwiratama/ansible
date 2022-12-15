---
title: "Cara Menyiapkan Klaster Ceph Menggunakan Ceph Deploy (Ubuntu 18.04 Bionic)"
description: "Cara menyiapkan klaster ceph rilis octopus menggunakan ceph deploy pada ubuntu 18.04 bionic"
date: "2021-11-07"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- Ubuntu 18.04 Bionic
- Ceph 15.2.X Octopus

# Panduan
1. Siapkan file hosts pada semua host
```bash
sudo bash -c 'cat<<EOF >> /etc/hosts

######## Lab Environment ########
192.168.10.211 aa-bionic-ceph01
192.168.10.212 aa-bionic-ceph02
192.168.10.213 aa-bionic-ceph03
#################################
EOF'
```

2. Update dan upgrade package pada semua host
```bash
sudo apt update -y; sudo apt upgrade -y
```

3. Pasang uca repository pada semua host
```bash
sudo add-apt-repository cloud-archive:ussuri
```

4. Pasang package ceph-deploy pada `aa-bionic-ceph01`
```bash
sudo apt install -y ceph-deploy
```

5. Pasang package-package ceph pada semua host 
```bash
mkdir ~/ceph-deploy; cd ~/ceph-deploy
ceph-deploy install aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
```

6. Inisiasi ceph monitor
```bash
ceph-deploy new aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
ceph-deploy mon create aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
```

7. Dapatkan keyring cluster
```bash
ceph-deploy gatherkeys aa-bionic-ceph01
```

Keyring yang akan didapatkan:
- ceph.bootstrap-mds.keyring
- ceph.bootstrap-mgr.keyring
- ceph.bootstrap-osd.keyring
- ceph.bootstrap-rgw.keyring
- ceph.client.admin.keyring
- ceph.mon.keyring

8. Persiapkan agar host dapat melakukan administrasi klaster 
Generate file ceph.conf dan ceph.client.admin.keyring
```bash
ceph-deploy admin aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
```

9. Membuat ceph manager
```bash
ceph-deploy mgr create aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
```

10. Membuat OSD
```bash
ceph-deploy osd create aa-bionic-ceph01 --data /dev/vdb
ceph-deploy osd create aa-bionic-ceph01 --data /dev/vdc
ceph-deploy osd create aa-bionic-ceph01 --data /dev/vdd

ceph-deploy osd create aa-bionic-ceph02 --data /dev/vdb
ceph-deploy osd create aa-bionic-ceph02 --data /dev/vdc
ceph-deploy osd create aa-bionic-ceph02 --data /dev/vdd

ceph-deploy osd create aa-bionic-ceph03 --data /dev/vdb
ceph-deploy osd create aa-bionic-ceph03 --data /dev/vdc
ceph-deploy osd create aa-bionic-ceph03 --data /dev/vdd
```

11. Hasil

![](/images/ceph-deploy-bionic.png)

# Referensi
- [Ceph Docs - ceph-deploy Ceph Node Setup](https://docs.ceph.com/en/octopus/install/ceph-deploy/quick-start-preflight/#ceph-node-setup)