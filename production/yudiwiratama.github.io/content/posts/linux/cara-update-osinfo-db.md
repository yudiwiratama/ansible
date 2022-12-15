---
title: "Cara Memperbarui libosinfo osinfo-db"
description: "Cara memperbarui libosinfo osinfo-db pada Linux"
date: "2021-09-05"
tags: ["Linux", "Virtualisasi"]
categories: ["Virtualisasi"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
**libosinfo** adalah penyedia informasi mengenai sistem operasi yang diperlukan untuk menyediakan dan mengelola _virtualized environment_ 

# Panduan
1. Perbarui _libosinfo_
    - Debian Based
    ```bash
    sudo apt install -y libosinfo
    ```

    - RHEL Based
    ```bash
    sudo yum install -y libosinfo
    # atau
    sudo dnf install -y libosinfo
    ```

2. Unduh database OS

https://releases.pagure.org/libosinfo/

```bash
# wget https://releases.pagure.org/libosinfo/osinfo-db-<version>.tar.xz
wget https://releases.pagure.org/libosinfo/osinfo-db-20210903.tar.xz
```

3. Perbarui database OS
```bash
sudo osinfo-db-import --system <database_os>
```

4. Verifikasi
```bash
osinfo-query os
```