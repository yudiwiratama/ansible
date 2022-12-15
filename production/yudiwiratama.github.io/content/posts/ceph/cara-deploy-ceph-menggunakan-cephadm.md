---
title: "Cara Deploy Ceph Cluster Menggunakan cephadm Pada Rocky Linux 8"
description: "Cara mudah deploy Ceph Cluster pada Rocky Linux 8"
date: "2021-08-22"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Prasyarat 
- Minimal 3 Nodes dengan 3 disk tambahan

# Panduan
1. Login sebagai root
```bash
sudo -i
```

2. Tambahkan host pada file /etc/hosts
```bash
vim /etc/hosts
```

* /etc/hosts
```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.10.201 rocky-ceph-node01
192.168.10.202 rocky-ceph-node02
192.168.10.203 rocky-ceph-node03
```

3. Update dan Upgrade package
```bash
dnf update; dnf upgrade
```

4. Install python3, lvm2, dan podman
```bash
dnf install -y python3 lvm2 podman
```

5. Unduh cephadm pada node pertama
```bash
curl --silent --remote-name --location https://github.com/ceph/ceph/raw/pacific/src/cephadm/cephadm
chmod +x cephadm
```

6. Pilih versi rilis lalu install cephadm dan ceph-common
```bash
./cephadm add-repo --release octopus
./cephadm install

# Install ceph-common (Agar bisa menjalankan: perintah ceph, rbd)
dnf install -y ceph-common

# Verifikasi
which cephadm
```

7. Bootstrap monitor pada node pertama
```bash
cephadm bootstrap --mon-ip 192.168.10.201
```

![](/images/cephadm-bootstrap-complete.png)

8. Tambahkan ssh key cephadm ke semua node
```bash
# Pada node pertama
cat /etc/ceph/ceph.pub

# Pada semua node
# Salin dan tempel konten ceph.pub ke ~/.ssh/authorized_keys
vim ~/.ssh/authorized_keys
```

9. Tambahkan node lainnya sebagai host ceph
```bash
# List host
ceph orch host ls

# Perbarui alamat dan tambahkan host label pada node pertama
ceph orch host set-addr rocky-ceph-node01 192.168.10.201
ceph orch host label add rocky-ceph-node01 _admin

# Tambahkan label _admin pada node lainnya
ceph orch host add rocky-ceph-node02 192.168.10.202 _admin
ceph orch host add rocky-ceph-node03 192.168.10.203 _admin

# Tambahkan label osd-node pada semua node
ceph orch host label add rocky-ceph-node01 osd-node
ceph orch host label add rocky-ceph-node02 osd-node
ceph orch host label add rocky-ceph-node03 osd-node

# Verifikasi host
ceph orch host ls
```

> **Catatan!**  
> label _admin akan membuat host bisa menjalankan cephadm shell

10. Tambahkan ceph-mon & ceph-mgr ke node lainnya
```bash
ceph orch apply mon --placement="rocky-ceph-node01,rocky-ceph-node02,rocky-ceph-node03"
ceph orch apply mgr --placement="rocky-ceph-node01,rocky-ceph-node02,rocky-ceph-node03"

# Verifikasi
ceph orch ps
ceph -s 
```

11. Deploy ceph-osd pada semua node
```bash
vim deploy-osd-node.yaml
```

* deploy-osd-node.yaml
```yaml
service_type: osd
service_id: demo-osd # service_id bisa diisi secara bebas
placement:
  label: "osd-node"
data_devices:
  paths:
    - /dev/vdb
    - /dev/vdc
    - /dev/vdd
```

Terapkan yaml yang sudah dibuat
```bash
ceph orch apply -i deploy-osd-node.yaml
```

12. Verifikasi status Ceph Cluster
```bash
ceph -s
ceph osd tree
```
![](/images/cephadm-deploy-osd-complete.png)

# Referensi
- [Dokumentasi Ceph - cephadm](https://docs.ceph.com/en/latest/cephadm/install/)
