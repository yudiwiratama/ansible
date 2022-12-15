---
title: "Cara Menghapus OSD [Manual]"
description: "Cara menghapus OSD dari klaster ceph" 
date: "2021-09-05"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Hentikan servis jika masih berjalan
```bash
sudo systemctl stop ceph-osd@<id>
```

2. Atur _state_ OSD sebagai _down_ dan _out_
```bash
ceph osd set out <id>
ceph osd set down <id>
```

3. Hapus OSD dari klaster ceph
```bash
# Hapus auth
ceph auth rm osd.<osd_id> 

# Hapus dari CRUSH Map
ceph osd crush remove osd.<osd_id> 

# Hapus dari OSD Map
ceph osd rm <osd_id> 
```

4. _Zap_ disk
```bash
# ceph-volume lvm zap <device_path> --destroy
ceph-volume lvm zap /dev/sdX --destroy
```

## Solusi (Workaround)
### Tidak bisa menjalankan ceph-volume lvm destroy
1. Catat informasi mengenai OSD yang ingin dihapus
    - block
    - osd_id
```bash
ceph-volume lvm list
```

2. Format Disk menggunakan `wipefs`
```bash
wipefs -fa <device_path>
```

3. Hapus LVM jika masih ada 
```bash
# Lihat device mapper 
dmsetup ls

# Hapus device mapper OSD yang ingin dihapus
dmsetup remove <device_map>
```

4. _Zap_ disk
```bash
# ceph-volume lvm zap <device_path> --destroy
ceph-volume lvm zap /dev/sdX --destroy
```

# Referensi
- [Ceph Docs - ceph-volume ZAP](https://docs.ceph.com/en/latest/ceph-volume/lvm/zap/)
- [The Geek Diary - dmsetup](https://www.thegeekdiary.com/examples-of-using-dmsetup-command-in-linux/)