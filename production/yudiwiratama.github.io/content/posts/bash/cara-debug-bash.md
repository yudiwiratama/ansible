---
title: "Cara Debug Bash Script"
description: "Cara mudah yang bisa dilakukan agar kita bisa memperbaiki atau debug bash script"
date: "2021-08-12"
tags: ["Bash", "Linux"]
categories: ["Bash"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Gunakan kata kunci `set` pada bash dengan opsi `-v`
```bash
#!/bin/bash

# Tambahkan set -v pada script
set -v
```

# Contoh
* `read.sh`
```bash
#!/bin/bash

# Baris berikut akan membuat bash script menjadi verbose
set -v

echo "Siapa namamu?"
read NAMA

echo "Berapa umurmu? (Dalam Tahun)"
read UMUR

# tampilkan nama
echo "${NAMA}"

# tampilkan umur
echho "${UMUR} Tahun"

# Sapa nama
echo "Halo, ${NAMA}!"
```

Ketika menjalankan `read.sh`
```bash
[student@debug-bash bash]$ bash read.sh

echo "Siapa namamu?"
Siapa namamu?
read NAMA
Dumbledore          <- ini input

echo "Berapa umurmu? (Dalam Tahun)"
Berapa umurmu? (Dalam Tahun)
read UMUR
150                 <- ini input

# tampilkan nama
echo "${NAMA}"
Dumbledore

# tampilkan umur
echho "${UMUR} Tahun"                         <- Ini penyebab error pada script (typo)
read.sh: line 16: echho: command not found    <- Ini pesan error ketika menjalankan script

# Sapa nama
echo "Halo, ${NAMA}!"
Halo, Dumbledore!
```