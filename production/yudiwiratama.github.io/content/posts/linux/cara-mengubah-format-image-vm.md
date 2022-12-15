---
title: "Cara Mengubah Format Image VM"
description: "Cara mudah mengubah format image vm"
date: "2021-08-29"
tags: ["Linux", "Virtualisasi", "KVM"]
categories: ["KVM"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Image yang ingin diubah
- Binary `qemu-img`

# Panduan

## Install qemu-img

- Ubuntu/Debian
```bash
sudo apt install -y qemu-img
```

- RHEL/CentOS/Rocky 7
```bash
sudo yum install -y qemu-img
```

- RHEL/CentOS/Rocky 8
```bash
sudo dnf install -y qemu-img
```

## Perintah
```bash
qemu-img convert -f <format_sekarang> -O <format_output> <image_sekarang> <image_output>
```

## Contoh
### qcow2 ke raw
```bash
qemu-img convert -f qcow2 -O raw contoh.qcow2 contoh.img
```

### raw ke qcow2
```bash
qemu-img convert -f raw -O qcow2 contoh.img contoh.qcow2
```

### vmdk ke qcow2
```bash
qemu-img convert -f vmdk -O qcow2 contoh.vmdk contoh.qcow2
```

# Referensi
- [OpenStack Docs - Image Guide](https://docs.openstack.org/id/image-guide/convert-images.html)