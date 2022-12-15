---
title: "Cara Membuat Image Libvirt Menggunakan python-tfgen"
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
- Storage Pool ([Baca disini untuk cara membuat Storage Pool](/posts/python-tfgen/cara-membuat-pool-dengan-python-tfgen/))

# Panduan
1. Buat yaml dengan `kind: image` lalu isikan dengan daftar image yang diinginkan
* image.yaml
```
kind: image
uri: "qemu:///system"
spec:
  - name: debian-bullseye
    url: "https://cdimage.debian.org/images/cloud/bullseye/latest/debian-11-genericcloud-amd64.qcow2"
    pool: images
  - name: ubuntu-focal
    url: "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img"
    pool: images
```

2. Buat HCL berdasarkan yaml yang telah dibuat
```bash
./tfgen.py -f image.yaml -o image
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

resource "libvirt_volume" "debian-bullseye" {
  name   = "debian-bullseye"
  source = "https://cdimage.debian.org/images/cloud/bullseye/latest/debian-11-genericcloud-amd64.qcow2"
  pool = "images"
}

resource "libvirt_volume" "ubuntu-focal" {
  name   = "ubuntu-focal"
  source = "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img"
  pool = "images"
}
```

3. Berpindah ke direktori yang dihasilkan python-tfgen
```bash
cd image/
```

4. Jalankan inisiasi terraform
```bash
terraform init
```

5. Jalankan terraform untuk membuat image
```bash
terraform apply
```
> **Tips!**  
> Gunakan `terraform apply -auto-approve` untuk melewati prompt persetujuan.

6. Verifikasi image yang telah terbuat
```bash
virsh vol-list images
```

![](/images/python-tfgen-image.jpg)

# Panduan Menghapus
```bash
terraform destroy
```