---
title: "Cara Konfigurasi SSH Config"
description: "Cara membuat jalan pintas untuk ssh"
date: "2021-08-20"
tags: ["Linux", "SSH"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Contoh SSH Config
```bash
host <nama_pintas>         # Nama pintasan
  hostname <alamat_tujuan> # Bisa IP Address ataupun domain
  user <user>              # Nama user untuk remote
  port <port>              # Port yang digunakan untuk SSH
```

# Panduan
1. Buat file ssh config
```bash
vim ~/.ssh/config
```

* ~/.ssh/config
```bash
host home-lab
  hostname 1.tcp.jp.ngrok.io
  user ubuntu
  port 2222
```

2. Lakukan SSH
```bash
ssh home-lab
```
> **Catatan**  
> Cara manual jika tanpa menggunakan ssh config: `ssh ubuntu@1.tcp.jp.ngrok.io -p2222`