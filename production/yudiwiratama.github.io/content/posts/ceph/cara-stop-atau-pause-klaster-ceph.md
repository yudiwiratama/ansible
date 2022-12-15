---
title: "Cara Stop atau Pause Klaster Ceph"
description: "Cara yang bisa dilakukan untuk menghentikan atau menjeda klaster Ceph"
date: "2021-09-04"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Saat kita ingin melakukan _Maintenance_ atau _Mematikan_ server sebaiknya kita menghentikan atau menjeda klaster Ceph yang sedang berjalan

# Panduan
## Menghentikan atau melakukan pause klaster
```bash
ceph osd set noout
ceph osd set norecover
ceph osd set norebalance
ceph osd set nobackfill
ceph osd set nodown
ceph osd set pause
```

## Menjalankan atau menghentikan pause klaster
```bash
ceph osd unset noout
ceph osd unset norecover
ceph osd unset norebalance
ceph osd unset nobackfill
ceph osd unset nodown
ceph osd unset pause
```

# Referensi
- [Red Hat Ceph Storage 3 - 2.6. Step No. 4](https://access.redhat.com/documentation/en-us/red_hat_ceph_storage/3/html/administration_guide/understanding-process-managemnet-for-ceph#powering-down-and-rebooting-a-red-hat-ceph-storage-cluster-management)
- [Manual Ceph Command](https://docs.ceph.com/en/latest/man/8/ceph/)
- [Ceph OSD Map Flags](https://docs.ceph.com/en/latest/rados/operations/health-checks/#osdmap-flags)