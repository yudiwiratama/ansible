---
title: "Cara Install ZFS Pada Linux [CentOS/Rocky]"
description: "Cara mudah install ZFS pada CentOS 8 atau Rocky 8"
date: "2021-08-09"
tags: ["Linux", "Filesystem", "ZFS"]
categories: ["ZFS"]
ShowToc: true
TocOpen: true
---

# Panduan
## Menambahkan repository dan public signing key
```bash
source /etc/os-release
sudo dnf install https://zfsonlinux.org/epel/zfs-release.el${VERSION_ID/./_}.noarch.rpm
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-zfsonlinux
```

## Install package ZFS
```bash
sudo yum-config-manager --disable zfs
sudo yum-config-manager --enable zfs-kmod
sudo dnf install epel-release kernel-devel zfs
```

## Verifikasi versi
```bash
modinfo zfs | grep version
```

# Referensi
* [OpenZFS Docs](https://openzfs.github.io/openzfs-docs/Getting%20Started/RHEL-based%20distro/index.html)