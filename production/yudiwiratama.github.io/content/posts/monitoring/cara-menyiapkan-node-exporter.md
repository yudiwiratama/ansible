---
title: "Cara Menyiapkan Node Exporter Pada Linux (CentOS/Ubuntu)"
description: "Cara mudah menyiapkan node exporter pada sistem operasi linux" 
date: "2021-10-07"
tags: ["Prometheus", "Visualisasi", "Monitoring"]
categories: ["Monitoring"]
ShowToc: true
TocOpen: true
---

# Panduan
## Menyiapkan Node Exporter
1. Kunjungi website prometheus untuk melihat versi node exporter yang tersedia
https://prometheus.io/download/#node_exporter

2. Buat user
```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

3. Unduh, ekstrak, dan pasang node exporter
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
tar xf node_exporter-1.2.2.linux-amd64.tar.gz
sudo cp node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin/node_exporter
```

4. Buat file servis `node_exporter`
```bash
sudo bash -c 'cat<<EOF > /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
EOF'
```

5. Jalankan servis `node_exporter`
```bash
sudo systemctl enable --now node_exporter
sudo systemctl status node_exporter
```

## Tambahkan konfigurasi pada prometheus.yaml
```bash
...
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
    - targets:
      - <ip_address>:9100
```