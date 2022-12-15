---
title: "Cara Upgrade Cluster Ceph Yang Menggunakan cephadm"
description: "Cara mudah upgrade ceph cluster yang menggunakan deployment tools cephadm"
date: "2021-08-22"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Periksa versi ceph yang sedang digunakan
```bash
ceph versions
```

2. Lihat rilis ceph dan baca catatan rilis (release note)

Kunjungi [Ceph Release](https://docs.ceph.com/en/latest/releases/index.html)

3. Lakukan upgrade
```bash
ceph orch upgrade start --ceph-version <versi_ceph>

# Contoh
# ceph orch upgrade start --ceph-version 16.2.5
```

4. Pantau proses upgrade  

Gunakan perintah-perintah dibawah untuk melakukan pengecekan pada proses upgrade
```bash
ceph orch upgrade status

ceph -W cephadm

ceph orch ps

ceph versions
```
![](/images/cephadm-upgrade-before.png)
ceph versions saat proses upgrade sedang berlangsung
 
5. Verifikasi upgrade
```bash
ceph -s

ceph versions
```
![](/images/cephadm-upgrade-after.png)
ceph versions saat proses upgrade selesai

# Referensi
- [Ceph Release](https://docs.ceph.com/en/latest/releases/index.html)
- [cephadm upgrade guide](https://docs.ceph.com/en/latest/cephadm/upgrade/)