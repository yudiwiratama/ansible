---
title: "Cara Memperbaiki Warning Monitors Have Not Enabled msgr2 Pada Ceph"
description: "Menghilangkan warning monitors have not enabled msgr2 pada ceph"
date: "2021-11-06"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

![](/images/ceph-mon-warning-msgr2-1.png)

# Penjelasan
warning ini muncul saat melakukan upgrade dari mimic > octopus. fitur msgr2 ada semenjak rilis Nautilus.

# Panduan
1. Aktifkan msgr2 pada ceph monitor
```bash
sudo ceph mon enable-msgr2
```

2. Hasil

![](/images/ceph-mon-warning-msgr2-2.png)

# Referensi
- [Docs Ceph - Configuration Messenger V2](https://docs.ceph.com/en/latest/rados/configuration/msgr2/#transitioning-from-v1-only-to-v2-plus-v1)