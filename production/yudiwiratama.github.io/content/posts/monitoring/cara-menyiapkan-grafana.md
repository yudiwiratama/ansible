---
title: "Cara Menyiapkan Grafana Pada Linux (CentOS/Ubuntu)"
description: "Cara mudah menyiapkan grafana pada sistem operasi linux" 
date: "2021-09-18"
tags: ["Grafana", "Visualisasi", "Monitoring"]
categories: ["Monitoring"]
ShowToc: true
TocOpen: true
---

# Panduan
## RHEL/CentOS/Rocky
1. Menyiapkan repository grafana
```bash
sudo bash -c 'cat<<EOF > /etc/yum.repos.d/grafana.repo
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
EOF
```

2. Pasang grafana
    - yum
    ```bash
    sudo yum install -y grafana
    ```

    - dnf
    ```bash
    sudo dnf install -y grafana
    ```

3. Jalankan dan aktifkan servis grafana
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now grafana-server
```

## Debian/Ubuntu
1. Menyiapkan repository grafana
```bash
# Menambahkan gpg key
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

# Pasang repo
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

2. Pasang grafana
```bash
sudo apt update
sudo apt install -y grafana
```


3. Jalankan dan aktifkan servis grafana
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now grafana
```


# Referensi
- [Setup Grafana - RPM Based](https://grafana.com/docs/grafana/latest/installation/rpm/)
- [Setup Grafana - Deb Based](https://grafana.com/docs/grafana/latest/installation/debian/)