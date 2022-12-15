---
title: "Ceph - Cara Migrasi Ceph Daemon ke cephadm"
description: "Cara mengubah klaster ceph yang menggunakan daemon menjadi ceph yang berbasiskan container dengan cephadm"
date: "2021-11-09"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- Ubuntu 20.04 Focal
- Ceph 15.2.X Octopus
- Podman 3.3.1

**Mesin**
| hostname         | ip address     |
|------------------|----------------|
| aa-bionic-ceph01 | 192.168.10.211 |
| aa-bionic-ceph02 | 192.168.10.212 |
| aa-bionic-ceph03 | 192.168.10.213 |

# Panduan
1. Masuk sebagai root
```bash
sudo -i
```

2. Pasang cephadm pada semua host
```bash
curl --silent --remote-name --location https://github.com/ceph/ceph/raw/octopus/src/cephadm/cephadm
chmod +x cephadm
./cephadm install

# verifikasi
dpkg -l cephadm
```

3. Migrasi ceph-mon, lakukan pada setiap host ceph-mon
```bash
cephadm adopt --style legacy --name mon.$(hostname)
```

4. Migrasi ceph-mgr, lakukan pada setiap host ceph-mgr
```bash
cephadm adopt --style legacy --name mgr.$(hostname)
```

5. Aktifkan module cephadm pada manager lakukan dari salah satu ceph-mon
```bash
ceph mgr module enable cephadm
ceph orch set backend cephadm
```

6. Buat ssh key untuk cephadm dari salah satu ceph-mon
```bash
ceph cephadm generate-key
ceph cephadm get-pub-key > ~/ceph.pub
```

7. Tambahkan public key ke host lain
```bash
ssh-copy-id -f -i ~/ceph.pub root@<host>

# atau salin tempel manual
## salin key
cat ~/ceph.pub

## tempel pada file authorized_keys
vi ~/.ssh/authorized_keys
```

8. Tambahkan mesin-mesin ke cephadm
```bash
ceph orch host add aa-bionic-ceph01 192.168.10.211
ceph orch host add aa-bionic-ceph02 192.168.10.212
ceph orch host add aa-bionic-ceph03 192.168.10.213
```

9. Migrasi ceph-osd, lakukan pada setiap host ceph-osd
```bash
# dapatkan id
ceph-volume lvm list | grep === | awk '{print $2}' | sed 's/osd.//g'

# migrasi osd satu per satu
cephadm adopt --style legacy --name osd.<osd_id>
cephadm adopt --style legacy --name osd.0
cephadm adopt --style legacy --name osd.1
cephadm adopt --style legacy --name osd.2
```

# Referensi
- [Ceph Docs - cephadm adoption process](https://docs.ceph.com/en/latest/cephadm/adoption/#adoption-process)