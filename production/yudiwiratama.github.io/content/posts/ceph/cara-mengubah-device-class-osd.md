---
title: "Cara Mengubah Device Class OSD"
description: "Cara mudah mengubah device class OSD" 
date: "2021-09-05"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Device Class berguna untuk mempermudah ceph untuk mendistribusikan data berdasarkan device class yang telah ditentukan didalam CRUSH Rule

# Panduan
1. Hapus class _default_ yang didapatkan ketika membuat OSD
```
ceph osd crush rm-device-class <osd_id>
```

2. Berikan class baru pada OSD yang diinginkan
```
ceph osd crush set-device-class performance $i
```

## Contoh memanfaatkan device class
```bash
# Buat CRUSH rule
# Contoh:
# ceph osd crush rule create-replicated <rule-name> <root> <failure-domain> <class>
ceph osd crush rule create-replicated replicated-performance default host performance

# Buat pool
ceph osd pool create performance-pool 32 32 replicated replicated-performance
```

# Referensi
- [Ceph News - Crush Device Classes](https://ceph.io/en/news/blog/2017/new-luminous-crush-device-classes/)