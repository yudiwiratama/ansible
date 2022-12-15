---
title: "Cara Membuat Ulang Ceph Monitor Yang Rusak [Manual Deployment]"
description: "Cara mudah membuat ulang monitor yang rusak pada ceph cluster"
date: "2021-08-24"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Jika mengalami kerusakan pada ceph monitor kita bisa melakukan hapus dan buat monitor yang rusak tersebut dengan ceph-monitor lainnya masih tersedia bagi cluster

# Panduan
1. Login sebagai root
```bash
sudo -i
```

2. Hentikan service monitor jika masih berjalan
```bash
systemctl stop ceph-mon@<monitor_name>
```

3. Hapus direktori monitor dan buat ulang direktori
```bash
rm -rf /var/lib/ceph/mon/ceph-<monitor_name>
mkdir /var/lib/ceph/mon/ceph-<monitor_name>
```

4. Remove monitor from the cluster
```bash
ceph mon rm <monitor_name>
```

5. Dapatkan keyring dan monmap (monitor map)
```bash
ceph auth get mon. -o /tmp/keyring
ceph mon getmap -o /tmp/monmap
```

6. Sunting monmap (monitor map)
```bash
monmaptool /tmp/monmap --add <monitor_name> <monitor_ip>
```

7. Buat ulang monitor yang telah dihapus
```bash
ceph-mon -i <monitor_name> --cluster ceph --mkfs --monmap /tmp/monmap --keyring /tmp/keyring
```

8. Ganti pemilik direktori monitor menjadi ceph
```bash
chown -R ceph:ceph /var/lib/ceph/mon/ceph-<monitor_name>
```

9. Jalankan service
```bash
systemctl daemon-reload
systemctl start ceph-mon@<monitor_name>
```

# Referensi
- [Ceph Docs - Adding/Removing Monitor](https://docs.ceph.com/en/mimic/rados/operations/add-or-rm-mons/)