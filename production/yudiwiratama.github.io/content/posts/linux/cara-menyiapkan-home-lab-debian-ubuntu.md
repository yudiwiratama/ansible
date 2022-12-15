---
title: "Cara Menyiapkan Home Lab Linux [Debian Bullseye/Ubuntu Focal]"
description: "Cara mudah menyiapkan home lab untuk riset"
date: "2021-08-17"
tags: ["Linux", "Virtualisasi", "KVM", "Cockpit"]
categories: ["Virtualisasi"]
ShowToc: true
TocOpen: true
---

![](/images/cara-menyiapkan-home-lab-debian.png)
Debian Cockpit

![](/images/cara-menyiapkan-home-lab-ubuntu.png)
Ubuntu Cockpit

# Referensi Sewa Server
- [Hetzner Robot - Server Auction](https://www.hetzner.com/sb)

# Prasyarat
- Server
- Sistem Operasi Debian 11 (Bullseye) atau Ubuntu 20.04 (Focal) terinstall

# Panduan
1. Update & Upgrade package 
```bash
sudo apt update
sudo apt upgrade
```

2. Install package-package untuk host virtualisasi
```bash
sudo apt install cockpit cockpit-machines qemu-system-x86 libvirt-daemon-system libvirt-clients bridge-utils
```

3. Jalankan dan enable servis libvirt
```bash
sudo systemctl enable --now libvirtd
```

4. Jalankan dan enable servis cockpit
```bash
sudo systemctl enable --now cockpit.socket
```

# Referensi
- [Cockpit Project - Debian](https://cockpit-project.org/running#debian)
- [Cockpit Project - Ubuntu](https://cockpit-project.org/running#ubuntu)

# Baca juga
- [Cara membuat Guest KVM dengan menggunakan Terraform Libvirt Provider](/posts/linux/cara-menggunakan-terraform-libvirt-provider/)