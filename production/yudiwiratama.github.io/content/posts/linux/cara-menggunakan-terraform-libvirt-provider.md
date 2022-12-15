---
title: "Cara Menggunakan Terraform Libvirt Provider Dengan Bantuan python-tfgen"
description: "Cara mudah install terraform dengan libvirt provider dan menggunakan python-tfgen untuk menghasilkan Terraform Configuration File"
date: "2021-08-17"
tags: ["Terraform", "Linux", "Virtualisasi", "KVM"]
categories: ["Automation"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Apa yang membuat kita malas untuk belajar atau mempraktikan ilmu IT? **Membuat Lab!** Tenang semua itu bisa dibuat tidak membosankan dan bisa dilakukan dengan cepat.

Saya menggunakan KVM Host untuk melakukan riset ataupun belajar hal baru, sehingga intensitas saya dalam membuat VM itu bisa dibilang sangat sering dan saya rasa membuat VM adalah hal membosankan yang berulang-ulang sehingga saya membuat sebuah __tools__ agar mempermudah dan mempercepat pembuatan VM.

# Panduan
1. Unduh Terraform
```bash
wget https://releases.hashicorp.com/terraform/1.0.4/terraform_1.0.4_linux_amd64.zip
```

2. Install Unzip
```bash
# Debian/Ubuntu
sudo apt install -y unzip

# CentOS / RHEL / Rocky
sudo dnf install -y unzip
## atau ##
sudo yum install -y unzip
```

3. Ekstrak file zip dan pindahkan binary file terraform
```bash
unzip terraform_1.0.4_linux_amd64.zip
mv terraform /usr/local/bin
```

# Daftar Panduan python-tfgen
## Blog
- [Pool](/posts/python-tfgen/cara-membuat-pool-dengan-python-tfgen/)
- [Image](/posts/python-tfgen/cara-membuat-image-dengan-python-tfgen/)
- [Network](/posts/python-tfgen/cara-membuat-network-dengan-python-tfgen/)
- [VM](/posts/python-tfgen/cara-membuat-vm-dengan-python-tfgen/)

# Sumber Kode
[GitHub python-tfgen](https://github.com/ajiarya/python-tfgen)
