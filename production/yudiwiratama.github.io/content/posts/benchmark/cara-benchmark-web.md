---
title: "Cara Benchmark Web Menggunakan hey"
description: "Cara benchmark web menggunakan hey" 
date: "2021-10-07"
tags: ["Benchmark", "Website"]
categories: ["Benchmark"]
ShowToc: true
TocOpen: true
---

# Panduan
![](/images/benchmark-web-hey.png)

## Jalankan dengan jumlah request
```bash
# 1000 request
hey -n 1000

# 500 request bersamaan dengan sejumlah 1000 request
hey -c 500 -n 1000
```

## Jalankan dengan durasi waktu
```bash
# 30 detik
hey -z 30s

# 5 menit
hey -z 5m

# 500 request bersamaan dengan durasi 5 menit
hey -c 500 -z 5m
```

# Referensi
- [GitHub - hey](https://github.com/rakyll/hey)
- [Man hey - Ubuntu](https://manpages.ubuntu.com/manpages/focal/man1/hey.1.html)