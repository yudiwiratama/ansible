---
title: "Cara Mengubah Volume Type Dengan Backend Yang Berbeda - CLI"
description: "Cara mudah mengubah volume type dengan backend yang berbeda pada service cinder openstack"
date: "2021-09-10"
tags: ["OpenStack", "Cinder", "Volume"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Cinder dengan 2 atau lebih backend

# Panduan
1. Lihat volume type yang tersedia
```bash
openstack volume type list
```

2. Ubah volume type pada volume yang dimaksud
```bash
openstack volume set --type <new_type> --retype-policy on-demand <vol_id/vol_name>
```

3. Tunggu sampai proses `retyping` selesai