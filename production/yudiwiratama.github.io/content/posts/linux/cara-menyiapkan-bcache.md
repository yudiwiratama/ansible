---
title: "Cara Menyiapkan Bcache"
description: "Cara menyiapkan bcache"
date: "2021-11-04"
tags: ["Linux", "Storage", "Bcache"]
categories: ["Bcache"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Bcache adalah teknologi cache terhadap _block device_ yang memungkinkan kita menggabungkan 2 perangkat SSD/NVMe dan HDD sehingga kita bisa mendapatkan performa baca tulis yang lebih baik. Caching akan dilakukan pada SSD (_Caching Device_) dan data tetap akan disimpan pada HDD (_Backing Device_).

# Glosari
- **Bcache:** Linux kernel block layer cache
- **Backing Device:** Tempat data disimpan (Biasanya HDD)
- **Cache Device:** Tempat cache disimpan (Biasanya SSD/NVMe)

# Panduan
1. Membuat bcache backing device
```bash
make-bcache -B /dev/vdb
```

2. Membuat bcache cache device
```bash
make-bcache -C /dev/vdc
```

Contoh output lsblk
```bash
root@home-lab:~# lsblk -f
NAME      FSTYPE   LABEL           UUID                                 FSAVAIL FSUSE% MOUNTPOINT
vda
├─vda1    ext4     cloudimg-rootfs e616a2cd-3c02-4c79-9823-9b1bb5c13b26   88.3G     9% /
├─vda14
└─vda15   vfat     UEFI            4411-1580                              97.8M     6% /boot/efi
vdb       bcache                   b5a16dff-9934-4653-8730-a78d84efd9d9
└─bcache0
vdc       bcache                   3243eb27-dac7-4e52-9d2e-a1fc48f1650b
vdd
```

3. Dapatkan `cset.uuid` cache device
```bash
bcache-super-show /dev/vdc | grep cset.uuid
```

Contoh output
```bash
root@home-lab:~# bcache-super-show /dev/vdc | grep cset.uuid
cset.uuid               bc80036c-ae27-4f38-a8ef-a4e06d9bf8e5
```

4. Daftarkan cache device pada backing device
```bash
echo bc80036c-ae27-4f38-a8ef-a4e06d9bf8e5 > /sys/block/bcache0/bcache/attach
```

Contoh output lsblk
```bash
root@home-lab:~# lsblk -f
NAME      FSTYPE   LABEL           UUID                                 FSAVAIL FSUSE% MOUNTPOINT
vda
├─vda1    ext4     cloudimg-rootfs e616a2cd-3c02-4c79-9823-9b1bb5c13b26   88.3G     9% /
├─vda14
└─vda15   vfat     UEFI            4411-1580                              97.8M     6% /boot/efi
vdb       bcache                   b5a16dff-9934-4653-8730-a78d84efd9d9
└─bcache0
vdc       bcache                   3243eb27-dac7-4e52-9d2e-a1fc48f1650b
└─bcache0
vdd
```
Jika backing device dan cache device memiliki nama bcache yang sama berarti pembuatan cache device sudah berhasil

5. Periksa mode bcache
```bash
cat /sys/block/bcache0/bcache/cache_mode
```

Contoh keluaran
```bash
root@home-lab:~# cat /sys/block/bcache0/bcache/cache_mode
[writethrough] writeback writearound none
```
Dari keluaran diatas mode yang sedang aktif adalah `writethrough`

## Mengubah Mode Bcache
Penjelasan mengenai mode-mode yang tersedia pada teknologi bcache

### Mode yang tersedia
- **writeback:** membaca dan menulis data dapat dilakukan pada cache lalu diteruskan ke backing device (Meningkatkan baca dan tulis | Performa terbaik namun terdapat kemungkinan kehilangan data ketika terjadi kerusakan perangkat) 
- **writethrough:** menulis data dapat dilakukan pada cache dan backing _device_ secara bersamaan sehingga proses penulisan bisa lebih baik karena dilakukan pada 2 device (Meningkatkan baca dan tulis | Aman dari kerusakan data)
- **writearound:** menulis data langsung pada backing device namun membaca dapat dilakukan pada caching device (Meningkatkan baca | Aman dari kerusakan data)
- **none:** tanpa cache (Cache tidak aktif namun tetap bisa menggunakan device bcache, misal /dev/bcacheX)

Baca lebih lanjut [disini](https://wiki.ubuntu.com/ServerTeam/Bcache)

1. Mengubah mode bache
```bash
echo <mode> > /sys/block/bcache0/bcache/cache_mode

# Example
# echo writeback > /sys/block/bcache0/bcache/cache_mode
 
```

# Referensi
- [Wiki Arch Linux - Bcache](https://wiki.archlinux.org/title/bcache)
- [Bcache caching modes](https://wiki.ubuntu.com/ServerTeam/Bcache)