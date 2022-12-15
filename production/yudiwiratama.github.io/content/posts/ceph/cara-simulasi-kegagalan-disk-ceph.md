---
title: "Cara Simulasi Kegagalan Disk Pada Klaster Ceph"
description: "Cara untuk uji coba kegagalan disk pada klaster Ceph" 
date: "2021-09-06"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Tentukan disk untuk simulasi
```bash
# Lihat OSD
ceph osd tree

# Temukan lokasi OSD
ceph osd find <osd_id>
```

2. Masuk ke Node OSD 
```bash
ssh ceph-osd-01
```

3. Hapus _device_ dari sysfs 
```bash
# echo 1 > /sys/block/BLOCK_DEVICE/device/delete
echo 1 > /sys/block/sdb/device/delete
```

4. Setelah beberapa saat, osd akan menjadi `down` dan kemudian menjadi `out`
```bash
ceph -s

ceph osd tree
ceph osd tree down
ceph osd tree out
```

## SCSI Scan (Untuk memunculkan SCSI yang belum terdeteksi oleh Sistem Operasi)
```bash
# Tampilkan host bus
ls /sys/class/scsi_host

# Scan ulang scsi berdasarkan host bus 
echo "- - -" > /sys/class/scsi_host/hostX/scan
```


# Referensi
- [Red Hat Ceph Storage 4 Docs - Handling a disk failure](https://access.redhat.com/documentation/en-us/red_hat_ceph_storage/4/html/operations_guide/handling-a-disk-failure)