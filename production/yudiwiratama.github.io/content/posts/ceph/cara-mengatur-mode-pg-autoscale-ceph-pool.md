---
title: "Cara Mengatur Mode PG Autoscale Pada Ceph Pool"
description: "Cara mengatur pg_autoscale_mode pada ceph pool" 
date: "2021-10-07"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Merubah pengaturan `pg_autoscale_mode` pada ceph pool yang sudah ada
```bash
ceph osd pool set <pool_name> pg_autoscale_mode [warn|on|off]
```

2. Cara mengatur konfigurasi default `pg_autoscale_mode` pada ceph pool
```bash
ceph config set global osd_pool_default_autoscale_mode [warn|on|off]
```

# Referensi
- [PG Merging dan Autotuning](https://ceph.io/en/news/blog/2019/new-in-nautilus-pg-merging-and-autotuning/)
