---
title: "Cara Konfigurasi DNS Server menggunakan dnsmasq"
description: "Cara konfigurasi dnsmasq sebagai dns server"
date: "2021-09-12"
tags: ["Linux", "DNS"]
categories: ["DNS"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
**dnsmasq** adalah software open source yang digunakan untuk menyediakan layanan seperti DNS, DCHP, TFTP, router advertisement, dan network boot. dnsmasq ditujukan untuk jaringan skala kecil - menengah.

# Panduan
1. Pasang dnsmasq
    - Debian Based
    ```bash
    sudo apt update
    sudo apt install -y dnsmasq
    ```

    - RHEL Based
    ```bash
    sudo yum update
    sudo yum install -y dnsmasq
    # atau
    sudo dnf update
    sudo dnf install -y dnsmasq
    ```

2. Buat file konfigurasi
```bash
# sudo vim /etc/dnsmasq.d/<name_file>.conf
sudo vim /etc/dnsmasq.d/contoh.conf
```

**Record**
- A = contoh.com 192.168.10.80
- CNAME = blog.contoh.com contoh.com
```ini
address=/contoh.com/192.168.10.80
cname=blog.contoh.com,contoh.com
```

**Upstream DNS/DNS Forwarding**
```ini
server=1.1.1.1
server=8.8.8.8
```

3. Restart servis
```bash
systemctl daemon-reload
systemctl restart dnsmasq
```

4. Uji coba
    1. Atur /etc/resolv.conf
    ```bash
    nameserver 192.168.10.80
    ```

    2. Permintaan DNS
    ```bash
    [root@dnshost ~]# nslookup blog.contoh.com
    Server:         192.168.10.80
    Address:        192.168.10.80#53
    
    blog.contoh.com canonical name = contoh.com.
    Name:   contoh.com
    Address: 192.168.10.80
    ```

# Referensi
- [man dnsmasq](https://linux.die.net/man/8/dnsmasq)
- [Server Fault - dnsmasq resolve cname](https://serverfault.com/questions/789530/resolve-a-domain-name-to-cname-alias-locally-using-dnsmasq/947713)
- [Wiki Arch - dnsmasqs](https://wiki.archlinux.org/title/dnsmasq)