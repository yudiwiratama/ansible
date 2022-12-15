---
title: "Cara Memperbaiki Warning Volumes Has Failed Dependencies Pada Ceph"
description: "Menghilangkan warning volumes has failed dependencies pada ceph"
date: "2021-11-06"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

![](/images/ceph-warning-volumes-1.png)

# Panduan
1. Pasang package `python3-distutil` pada mesin-mesin ceph-mgr
```bash
sudo apt install python3-distutils
```

2. Restart ceph-mgr secara satu per satu
```bash
sudo systemctl restart ceph-mgr.target
```

3. Hasil

![](/images/ceph-warning-volumes-2.png)

# Referensi
- [Proxmox Forum Thread](https://forum.proxmox.com/threads/proxmox-and-ceph-module-volumes-has-failed-dependency-no-module-named-distutils-util.79709/)