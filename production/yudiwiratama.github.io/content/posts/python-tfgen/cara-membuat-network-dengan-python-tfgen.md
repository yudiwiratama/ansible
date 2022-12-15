---
title: "Cara Membuat Network Libvirt Menggunakan python-tfgen"
description: "python-tfgen"
date: "2021-08-17"
tags: ["Terraform", "Linux", "Virtualisasi", "KVM", "python-tfgen"]
categories: ["Automation"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- [Terraform](/posts/linux/cara-menggunakan-terraform-libvirt-provider/)
- [python-tfgen](https://github.com/ajiarya/python-tfgen)

# Panduan
1. Buat yaml dengan `kind: network` lalu isikan dengan daftar network yang diinginkan
* network.yaml
```
kind: network
uri: "qemu:///system"
spec:
  - name: home-lab-network
    mode: nat
    bridge: home-lab-bridge
    dhcp: true
    addresses4: "192.168.100.0/24"
```

2. Buat HCL berdasarkan yaml yang telah dibuat
```bash
./tfgen.py -f network.yaml -o network
```

* image/main.tf (Hasil tfgen)
```hcl
terraform {
  required_providers {
    libvirt = {
      source = "dmacvicar/libvirt"
    }
  }
}

provider "libvirt" {
    uri = "qemu:///system"
}

resource "libvirt_network" "home-lab-network" {
  name       = "home-lab-network"
  mode       = "nat"
  bridge     = "home-lab-bridge"
  mtu        = 1500
  autostart  = true
  addresses = ["192.168.100.0/24"]
  dhcp {
    enabled = true
  }
}
```

3. Berpindah ke direktori yang dihasilkan python-tfgen
```bash
cd network/
```

4. Jalankan inisiasi terraform
```bash
terraform init
```

5. Jalankan terraform untuk membuat network
```bash
terraform apply
```
> **Tips!**  
> Gunakan `terraform apply -auto-approve` untuk melewati prompt persetujuan.

6. Verifikasi network yang telah terbuat
```bash
virsh net-list
```

![](/images/python-tfgen-network.jpg)

# Panduan Menghapus
```bash
terraform destroy
```