---
title: "Cara Melakukan Tune Sistem Operasi Dengan tuned-adm"
description: "Cara mudah melakukan tune sistem operasi Dengan tuned-adm"
date: "2022-06-28"
tags: ["Linux", "Tune"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Sistem Operasi
## Debian / Ubuntu
1. Pasang tuned-adm
```bash
apt install -y tuned
```

2. Start & enable servis tuned
```bash
systemctl enable --now tuned
```

3. Atur profil tuning 
```bash
# daftar profil yang bisa digunakan
tuned-adm list

# terapkan profil
tuned-adm profile <profile_name>
```

4. Verifikasi profil
```bash
tuned-adm active
```

## Red Hat / CentOS
1. Pasang tuned-adm
```bash
# using yum
yum install -y tuned

# using dnf
dnf install -y tuned
```

2. Start & enable servis tuned
```bash
systemctl enable --now tuned
```

3. Atur profil tuning 
```bash
# daftar profil yang bisa digunakan
tuned-adm list

# terapkan profil
tuned-adm profile <profile_name>
```

4. Verifikasi profil
```bash
tuned-adm active
```


# Referensi
- [Fedora Docs - tuned](https://docs.fedoraproject.org/en-US/Fedora/20/html/Power_Management_Guide/sect-tuned-installation-and-usage.html)
- [Manual tuned-adm](https://www.systutorials.com/docs/linux/man/8-tuned-adm/)