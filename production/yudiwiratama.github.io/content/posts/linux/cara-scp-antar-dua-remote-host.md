---
title: "Cara SCP Antar Dua Host Remote"
description: "Cara mudah mengirimkan file antara dua remote host melalui local host"
date: "2021-08-10"
tags: ["Linux", "SCP"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Cara ini digunakan agar kita tidak melakukan hal yang membosankan seperti kejadian berikut

> Conor ingin mengirimkan file dari host1 ke host2 dan hal yang dilakukannya adalah mengirimkan file dari host1 ke localhost terlebih dahulu dan lalu mengirimkan file dari localhost ke host2
1. Conor menyalin file `file100Giga` dari host1 ke localhost (Misal 15 menit)
```bash
scp host1:file100Giga file100Giga
```

2. Conor menyalin file `file100Giga` dari localhost ke host2 (Misal 15 menit)
```bash
scp file100Giga host1:file100Giga 
```

Total waktu yang dihabiskan 30 menit untuk memindahkan file dari host1 ke host2, agar lebih efisien waktu kita bisa memanfaatkan opsi `-3` pada `scp`. Ikuti panduan berikut

# Prasyarat
* localhost bisa mengakses kedua remote host

# Panduan
```bash
scp -3 <user>@<remote_host_1>:/file/yang/ingin/dikirimkan <user>@<remote_host_2>:/lokasi/pengiriman/file
```

* Contoh penggunaan
Mengirimkan file dari user student pada 192.168.10.10 ke user student pada 192.168.10.20
```bash
scp -3 student@192.168.10.10:/home/student/filecontoh student@192.168.10.20:/home/student/filecontoh-copy
```

# Referensi
* [man scp(1)](https://man7.org/linux/man-pages/man1/scp.1.html)