---
title: "Cara Mengetahui Virtualisasi Yang Digunakan oleh VM Linux"
description: "Cara mudah mengetahui virtualisasi yang digunakan oleh VM Linux"
date: "2021-08-04"
tags: ["Linux", "Virtualisasi"]
categories: ["Virtualisasi"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Ketika kita menggunakan layanan VPS ataupun Public Cloud kita bisa memeriksa jenis virtualisasi yang diberikan oleh penyedia layanan dengan menggunakan perintah `dmidecode` pada sistem operasi Linux.

# Panduan
```bash
sudo dmidecode -s system-product-name
```

* Contoh pada virtualisasi KVM
```bash
student@machine:~$ sudo dmidecode -s system-product-name
KVM
```

* Contoh pada virtualisasi OpenStack
```bash
student@machine:~$ sudo dmidecode -s system-product-name
OpenStack Compute
```