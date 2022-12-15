---
title: "Cara Membuat Storage Pool Libvirt Menggunakan python-tfgen"
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
1. Buat direktori untuk penyimpanan data
```bash
mkdir -p /data/images
mkdir -p /data/vms
```
> **Tips!**  
> Mount direktori ke drive dengan kapasitas besar atau kecepatan tinggi.

2. Buat yaml dengan `kind: pool`
* pool.yaml
```
kind: pool
uri: "qemu:///system"
spec:
  - name: images
    path: /data/images
  - name: vms
    path: /data/vms
```

3. Buat HCL berdasarkan yaml yang telah dibuat
```bash
./tfgen.py -f pool.yaml -o pool
```

* pool/main.tf (Hasil tfgen)
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

resource "libvirt_pool" "images" {
  name = "images"
  type = "dir"
  path = "/data/images"
}

resource "libvirt_pool" "vms" {
  name = "vms"
  type = "dir"
  path = "/data/vms"
}
```

4. Berpindah ke direktori yang dihasilkan python-tfgen
```bash
cd pool/
```

5. Jalankan inisiasi terraform
```bash
terraform init
```

6. Jalankan terraform untuk membuat storage pool
```bash
terraform apply
```
> **Tips!**  
> Gunakan `terraform apply -auto-approve` untuk melewati prompt persetujuan.

7. Verifikasi storage pool yang telah terbuat
```bash
virsh pool-list
```

![](/images/python-tfgen-pool.jpg)

# Panduan Menghapus
```bash
terraform destroy
```