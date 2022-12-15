---
title: "Cara Menyiapkan Klaster Ceph Pada Proxmox 7"
description: "Cara membuat klaster ceph pada proxmox 7"
date: "2021-09-13"
tags: ["Virtual Environment", "Proxmox", "Virtualisasi", "Storage", "Ceph"]
categories: ["Proxmox"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Proxmox terinstall

# Panduan
## Bootstrap ceph
1. Buka menu ceph
    1. Pilih menu Datacenter
    2. Pilih menu Ceph
    3. Klik "Install Ceph"

![](/images/proxmox-ceph-1.png)

2. Pilih versi ceph
    1. Pilih ceph version
    2. Klik "Start pacific installation"

![](/images/proxmox-ceph-2.png)

3. Konfirmasi instalasi
    1. Ketik "Y". Lalu tekan tombol enter
    2. Klik "Next"

![](/images/proxmox-ceph-3.png)

4. Konfigurasi klaster ceph
    1. Public Network
    2. Cluster Network
    3. Jumlah replika
    4. Minimum replika
    5. Monitor node pertama

![](/images/proxmox-ceph-4.png)

5. Instalasi sukses. Klik "Finish"

![](/images/proxmox-ceph-5.png)

## Tambah ceph-mon
1. Install package ceph

![](/images/proxmox-ceph-install-ceph.png)

2. Buat monitor tambahan
    1. Datacenter > Pilih Host
    2. Pilih menu Ceph
    3. Pilih menu Monitor
    4. Menu Monitor > Klik "Create"

![](/images/proxmox-ceph-6.png)

3. Pilih host untuk menempatkan ceph-mon baru
    1. Pilih Host
    2. Klik "Create"

![](/images/proxmox-ceph-7.png)

## Tambah ceph-mgr
1. Buat monitor tambahan
    1. Datacenter > Pilih Host
    2. Pilih menu Ceph
    3. Pilih menu Manager
    4. Bagian Manager > Klik "Create"

![](/images/proxmox-ceph-8.png)

2. Pilih host untuk menempatkan ceph-mgr baru
    1. Pilih Host
    2. Klik "Create"

![](/images/proxmox-ceph-9.png)

## Tambah ceph-osd
1. Buat osd
    1. Datacenter > Pilih Host
    2. Pilih menu Ceph
    3. Pilih menu OSD
    4. Klik "Create"

![](/images/proxmox-ceph-10.png)

2. Pilih host untuk menempatkan ceph-mgr baru
    1. Pilih Disk
    2. Pilih Device Class
    3. Klik "Create"

![](/images/proxmox-ceph-11.png)

3. Hasil

![](/images/proxmox-ceph-12.png)