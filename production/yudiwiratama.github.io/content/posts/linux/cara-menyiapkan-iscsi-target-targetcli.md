---
title: "Cara Menyiapkan ISCSI Target - targetcli (Linux)"
description: "Cara mudah menyediakan layanan iSCSI menggunakan tool targetcli"
date: "2021-09-04"
tags: ["Linux", "ISCSI", "Storage"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
**iSCSI Target** adalah server yang memiliki sumber daya penyimpanan dan penyimpanan tersebut bisa bisa digunakan oleh iSCSI Initiator

**targetcli** adalah _tool_ manajemen [LIO (Linux IO)](http://linux-iscsi.org/wiki/LIO). 

# Panduan
1. Install _package_ targetcli
    - Debian Distribution
    ```bash
    sudo apt install -y targetcli-fb
    ```

    - RHEL Distribution
    ```bash
    sudo dnf install -y targetcli
    # atau
    sudo yum install -y targetcli
    ```

2. Buat _backstore_  
**backstore** adalah sumber daya penyimpanan yang akan disediakan dan bisa gunakan oleh iSCSI Initiator. Kita bisa gunakan **fileio** untuk backstore yang berupa file ataupun **block** untuk backstore yang berupa _device_.
   - File
   1. Masuk shell targetcli
   ```bash
   sudo targetcli
   ``` 

   2. (Dalam targetcli) berpindah ke direktori backstores/fileio
   ```bash
   cd backstores/fileio
   ```

   3. (Dalam targetcli) Buat file yang akan dijadikan backstore
   ```bash
   # create <nama> <lokasi_file>
   create file-disk01 /data/file-disk01.img 10G
   ```

   4. (Dalam targetcli) Verifikasi
   ```bash
   cd /
   ls
   ```

   5. (Dalam targetcli) Keluar shell
   ```bash
   exit
   ```

   - Block
   1. Masuk shell targetcli
   ```bash
   sudo targetcli
   ``` 

   2. (Dalam targetcli) berpindah ke direktori backstores/block
   ```bash
   cd backstores/block
   ```

   3. (Dalam targetcli) Gunakan device yang akan dijadikan backstore
   ```bash
   # create <nama> <lokasi_device>
   create block-disk01 /dev/sdb
   ```

   4. (Dalam targetcli) Verifikasi
   ```bash
   cd /
   ls
   ```

   5. (Dalam targetcli) Keluar shell
   ```bash
   exit
   ```

3. Buat iSCSI Portal
   1. Masuk shell targetcli
   ```bash
   sudo targetcli
   ``` 

   2. (Dalam targetcli) berpindah ke direktori iscsi
   ```bash
   cd iscsi
   ```

   3. (Dalam targetcli) Buat iscsi
       - [Baca Standar IQN](https://datatracker.ietf.org/doc/html/rfc3720#section-3.2.6.3.1)
   ```bash
   # Buat Target
   # iqn.<YYYY-MM>.<reverse_domain>:<nama_target>
   create iqn.2021-08.aa-lio-target:lio.target01

   # Daftarkan backstore untuk dijadikan LUN
   cd iqn.2021-08.aa-lio-target:lio.target01/tpg1/luns
   create /backstores/fileio/file-disk01

   # Buat hak akses agar initiator bisa terhubung (Access List)
   cd ../acls
   create iqn.2021-08.aa-lio-initiator:lio.initiator01

   # Setel user dan password untuk initiator
   cd iqn.2021-08.aa-lio-initiator:lio.initiator01
   set auth userid=root
   set auth password=<password_rahasia>
   ```

# Referensi
- [Server World - iSCSI : Configure Target (Targetcli)](https://www.server-world.info/en/note?os=Debian_11&p=iscsi&f=1)
