---
title: "Cara Melakukan Percabangan Dasar Bash - Bagian 2 [Periksa Direktori, File, & Variabel]"
description: "Cara mudah melakukan percabangan pada bash script bagian 2"
date: "2021-08-23"
tags: ["Bash", "Linux"]
categories: ["Bash"]
ShowToc: true
TocOpen: true
---

# Panduan
## Parameter yang bisa digunakan
- `-d` _true_ jika direktori ada
- `-e` _true_ jika file ada
- `-v` _true_ jika variabel memiliki nilai

## Contoh 1: Periksa Direktori
* Script
```bash
#!/bin/bash
if [[ -d halo ]]; then
  echo "Direktori ada"
  else
  echo "Direktori tidak ada"
fi
```
![](/images/perulangan-bagian2-direktori.png)

## Contoh 2: Periksa File
* Script
```bash
#!/bin/bash
if [[ -e halo.txt ]]; then
  echo "File ada"
  else
  echo "File tidak ada"
fi
```
![](/images/perulangan-bagian2-file.png)

## Contoh 3: Periksa Variabel
* Script
```bash
#!/bin/bash

export VAR1="Halo Dunia"

if [[ -v VAR1 ]]; then
  echo "Variabel ada"
  echo $VAR1
  else
  echo "Variabel tidak ada"
  echo $VAR1
fi
```
![](/images/perulangan-bagian2-variabel.png)

# Referensi
- [GNU Org - Bash Manual](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)