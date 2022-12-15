---
title: "Cara Menyiapkan Ceph Tiering"
description: "Cara menyiapkan ceph tiering" 
date: "2021-10-07"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Untuk menyiapkan ceph tiering kita harus menyiapkan 2 pool. 1 sebagai penyimpanan data tetap dan 1 sebagai cache.

# Panduan
1. Buat crush rule untuk hdd dan ssd
```bash
ceph osd crush rule create-replicated replicated_hdd default host hdd
ceph osd crush rule create-replicated replicated_ssd default host ssd
```

2. Buat pool untuk hdd dan ssd
```bash
ceph osd pool create hot-storage 32 32 replicated replicated_ssd
ceph osd pool create cold-storage 32 32 replicated replicated_hdd
ceph osd pool application enable hot-storage rbd
ceph osd pool application enable cold-storage rbd
```

3. Tambahkan tier, atur cache-mode, dan atur overlay
```bash
ceph osd tier add cold-storage hot-storage
ceph osd tier cache-mode hot-storage writeback
ceph osd tier set-overlay cold-storage hot-storage
```

4. Atur `hit_set_type`
```bash
ceph osd pool set hot-storage hit_set_type bloom
```

# Referensi
- [Ceph Pools](https://docs.ceph.com/en/octopus/rados/operations/pools/)
- [Ceph Tiering](https://docs.ceph.com/en/latest/rados/operations/cache-tiering/)