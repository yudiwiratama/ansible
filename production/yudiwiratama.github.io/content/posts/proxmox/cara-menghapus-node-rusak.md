---
title: "Cara Menghapus Node Rusak Pada Proxmox"
description: "Cara menghapus node yang mengalami rusak atau kegagalan pada proxmox"
date: "2021-09-18"
tags: ["Virtual Environment", "Proxmox", "Virtualisasi", "Troubleshoot"]
categories: ["Proxmox"]
ShowToc: true
TocOpen: true
---

# Panduan
1. SSH salah satu node yang masih bisa diakses
```bash
ssh <running_pve_node>
```

2. Jalankan perintah berikut untuk melakukan penghapusan node
```bash
pvecm delnode <node_name>
```

3. Hapus direktori konfigurasi node tersebut
```bash
rm -rf /etc/pve/nodes/<node_name>
```

4. Muat ulang dashboard proxmox

# Referensi
- [Proxmox Forum](https://forum.proxmox.com/threads/removal-of-failed-node.41925/)