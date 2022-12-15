---
title: "Cara Menghitung Jumlah PG Untuk Klaster Ceph"
description: "Cara mudah untuk menghitung jumlah PG untuk klaster Ceph"
date: "2021-09-04"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Klaster Ceph

# Pendahuluan
Ceph Placement Groups (PGs) adalah implementasi internal yang dilakukan oleh Ceph untuk melakukan distribusi data

![](https://docs.ceph.com/en/latest/_images/ditaa-a53f413321ac943aa924fb6b58c0b3716821f6a7.png)
Mapping PG pada Pool



![](https://docs.ceph.com/en/latest/_images/ditaa-9d7f8f18e202e8cc5e09f9691de9266064af43b6.png)
Gambaran Pool yang menggunakan replikasi dengan nilai 2

Sumber Gambar: https://docs.ceph.com/en/latest/rados/operations/placement-groups/

# Panduan
1. Gunakan perhitungan 50-100 PG untuk 1 OSD
2. Hitung jumlah OSD pada klaster
3. Jumlah Replikasi yang diinginkan
4. Gunakan Kalkulator PG
    - [PGCalc](https://old.ceph.com/pgcalc/)

# Contoh
1. Klaster Ceph untuk servis OpenStack
2. Klaster memiliki 100 OSD dengan ukuran 1 TB setiap OSD (100 TB Total)
3. Target 100 PG untuk 1 OSD
4. Replikasi data sebanyak 3 (Size)
5. Alokasi pool
    - cinder-backup  25% (25 TB)
    - cinder-volumes 53% (53 TB)
    - ephemeral-vms  15% (15 TB)
    - glance-vms   7% (7 TB)

![](/images/pgcalc.png)
- **Pool Name:** Nama Pool
- **Size:** Ukuran replikasi atau EC (K+M)
- **OSD:** Jumlah OSD untuk pool tersebut (Berdasarkan OSD _device class_ untuk pool)
- **Target PGs Per OSD:** Target PG untuk 1 OSD (Rekomendasi 50-100 PG)
- **Suggested PG Count:** Saran PG merupakan kelipatan 2, Jumlah PG untuk pool

# Referensi
- [Ceph Docs - Placement Groups](https://docs.ceph.com/en/latest/rados/operations/placement-groups/)
