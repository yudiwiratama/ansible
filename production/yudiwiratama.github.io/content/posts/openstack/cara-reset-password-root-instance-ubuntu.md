---
title: "Cara Reset Password Root Dari OpenStack Console [Ubuntu]"
description: "Cara reset password root dari openstack console untuk instance yang menggunakan image ubuntu"
date: "2021-08-29"
tags: ["OpenStack", "Instance"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- OpenStack User
- Horizon
- Instance Ubuntu

# Panduan
1. Buka console instance pada horizon

2. Masuk ke boot option dengan cara interupsi proses booting

    - Reboot instance dan tekan tombol `Shift` keyboard pada saat proses reboot

3. Saat masuk boot option pilih kernel dan tekan tombol `e` pada keyboard

    - Cari baris dengan awalan `linux /boot/vmlinuz-xxxxxxxx`
    - Temukan `ro quiet` dan ganti dengan `rw init=/bin/bash`
    - Hapus kata berikut `console=ttyS0`
    - Lalu tekan `Ctrl + x`

4. Reset password root menggunakan perintah `passwd`

    - Jika sudah masuk maka tampilan terminal adalah sebagai berikut
    ```bash
    root@(none):/#
    ```

    - Ganti password 
    ```bash
    passwd
    ```

5. Reboot sistem menggunakan perintah berikut
```bash
exec /sbin/init 
```

# Referensi
- [Reset OpenStack Instance Password](https://jpenatech.wordpress.com/2017/04/20/successfully-resetting-the-root-password-of-a-centos-7-vm-in-openstack/)
- [Reset Root Password Ubuntu](https://www.tecmint.com/reset-forgotten-root-password-in-ubuntu/)