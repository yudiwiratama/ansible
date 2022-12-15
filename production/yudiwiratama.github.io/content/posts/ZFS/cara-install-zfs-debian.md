---
title: "Cara Install ZFS Pada Linux [Debian]"
description: "Cara mudah install ZFS pada Debian Buster"
date: "2021-08-09"
tags: ["Linux", "Filesystem", "ZFS"]
categories: ["ZFS"]
ShowToc: true
TocOpen: true
---

# Panduan
## Menambahkan backports repository
### Tambahkan repository
```bash
sudo vi /etc/apt/sources.list.d/buster-backports.list
```

* /etc/apt/sources.list.d/buster-backports.list
```
deb http://deb.debian.org/debian buster-backports main contrib
deb-src http://deb.debian.org/debian buster-backports main contrib
```

### Tambahkan konfigurasi APT
```bash
sudo vi /etc/apt/preferences.d/90_zfs
```

* /etc/apt/preferences.d/90_zfs
```
Package: libnvpair1linux libuutil1linux libzfs2linux libzpool2linux spl-dkms zfs-dkms zfs-test zfsutils-linux zfsutils-linux-dev zfs-zed
Pin: release n=buster-backports
Pin-Priority: 990
```

## Install package ZFS
### Install package
```bash
sudo apt update
sudo apt install dpkg-dev linux-headers-$(uname -r) linux-image-amd64
sudo apt install zfs-dkms zfsutils-linux
```

### Buat konfigurasi ZFS untuk kernel module load
```bash
sudo sh -c "echo zfs> /etc/modules-load.d/zfs.conf"
```

## Verifikasi versi
```bash
modinfo zfs | grep version
```

# Referensi
* [OpenZFS Docs](https://openzfs.github.io/openzfs-docs/Getting%20Started/Debian/index.html)