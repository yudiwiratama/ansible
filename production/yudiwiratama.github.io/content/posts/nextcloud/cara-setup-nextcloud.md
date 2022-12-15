---
title: "Cara Menyiapkan Nextcloud Dengan Let's Encrypt"
description: "Cara mudah setup nextcloud menggunakan snap"
date: "2021-09-05"
tags: ["Nextcloud", "DMS"]
categories: ["Nextcloud"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Pasang snap (Jika belum tersedia)
    - Debian Based
    ```bash
    sudo apt install -y snapd
    ```
    - RHEL Based
    ```bash
    sudo yum install -y epel-release
    sudo yum install -y snapd
    # atau
    sudo dnf install -y epel-release
    sudo dnf install -y snapd
    ```

    ```bash
    sudo systemctl enable --now snapd.socket
    sudo ln -s /var/lib/snapd/snap /snap
    ```

2. Pasang `Nextcloud` melalui snapd
```bash
sudo snap install nextcloud
```

3. Konfigurasi Akun administratif
```bash
sudo nextcloud.manual-install <username> <password>
```

4. Konfigurasi nama domain
```bash
# sudo nextcloud.occ config:system:set trusted_domains 1 --value=<alamat_domain>
sudo nextcloud.occ config:system:set trusted_domains 1 --value=contoh.com
```

5. _Generate_ SSL menggunakan Let's Encrypt
```bash
sudo nextcloud.enable-https lets-encrypt

# Verifikasi
sudo nextcloud.occ config:system:get trusted_domainsc
```

6. Buka firewall
    - ufw
    ```bash
    sudo ufw allow 80,443/tcp
    ```

    - firewalld
    ```bash
    firewall-cmd --permanent --zone=public --add-service=http
    firewall-cmd --permanent --zone=public --add-service=https
    firewall-cmd --reload
    ```
# Referensi
- [Snapcraft - Installing Snap on CentOS](https://snapcraft.io/docs/installing-snap-on-centos)
- [DigitalOcean - How To Install and Configure Nextcloud](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-nextcloud-on-ubuntu-18-04)