---
title: "Cara Menghilangkan warning insecure global_id reclaim Pada Ceph"
description: "Cara mudah menghilangkan warning insecure global_id reclaim" 
date: "2021-10-07"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

![](/images/ceph_insecure_global_id.png)

# Panduan
1. Perintah untuk mematikan fitur insecure global_id reclaim
```bash
ceph config set mon auth_expose_insecure_global_id_reclaim false
ceph config set mon auth_allow_insecure_global_id_reclaim false
```

# Referensi
- [CEPH - CVE-2021-20288](https://docs.ceph.com/en/latest/security/CVE-2021-20288/)
