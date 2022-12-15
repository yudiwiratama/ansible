---
title: "Cara Menyiapkan Home Lab Linux [CentOS/Rocky 8]"
description: "Cara mudah menyiapkan home lab untuk riset"
date: "2021-08-17"
tags: ["Linux", "Virtualisasi", "KVM", "Cockpit"]
categories: ["Virtualisasi"]
ShowToc: true
TocOpen: true
---

![](/images/cara-menyiapkan-home-lab-centos-rocky.png)
# Referensi Sewa Server
- [Hetzner Robot - Server Auction](https://www.hetzner.com/sb)

# Prasyarat
- Server
- Sistem Operasi CentOS 8 atau Rocky 8 terinstall

# Panduan
1. Update & Upgrade Package 
```bash
sudo dnf update
sudo dnf upgrade
```

2. Install grup package untuk host virtualisasi
```bash
sudo dnf groupinstall -y "Virtualization Host"
```

3. Jalankan dan enable servis libvirt
```bash
sudo systemctl enable --now libvirtd
```

4. Install cockpit plugin untuk VM
```bash
sudo dnf install -y cockpit-machines
```

5. Jalankan dan enable servis cockpit
```bash
sudo systemctl enable --now cockpit.socket
```

6. Buka port tcp/9090 firewall 
```bash
sudo firewall-cmd --permanent --zone=public --add-service=cockpit
sudo firewall-cmd --reload
```

# Referensi
- [Cockpit Project - CentOS](https://cockpit-project.org/running#centos)

# Baca juga
- [Cara membuat Guest KVM dengan menggunakan Terraform Libvirt Provider](/posts/linux/cara-menggunakan-terraform-libvirt-provider/)