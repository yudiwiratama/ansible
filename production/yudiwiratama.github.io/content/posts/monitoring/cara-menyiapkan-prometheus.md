---
title: "Cara Menyiapkan Prometheus Pada Linux"
description: "Cara mudah setup prometheus pada linux sebagai servis systemd" 
date: "2021-09-18"
tags: ["Prometheus", "Metric", "Monitoring"]
categories: ["Monitoring"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Buat user prometheus
```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

2. Unduh dan pasang prometheus
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.30.0/prometheus-2.30.0.linux-amd64.tar.gz
tar xf prometheus-2.30.0.linux-amd64.tar.gz
sudo cp prometheus-2.30.0.linux-amd64/prometheus /usr/local/bin/prometheus
```

3. Buat konfigurasi prometheus
```bash
sudo mkdir /etc/prometheus
sudo bash -c 'cat <<EOF > /etc/prometheus/prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: 
      - localhost:9090
EOF'
```

4. Buat file systemd untuk prometheus
```bash
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
sudo bash -c 'cat<<EOF > /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/ \
  --storage.tsdb.max-block-duration=2h \
  --storage.tsdb.min-block-duration=2h \
  --web.enable-admin-api \
  --web.enable-lifecycle

[Install]
WantedBy=multi-user.target
EOF'
```

5. Jalankan dan periksa status servis
```bash
sudo systemctl enable --now prometheus
sudo systemctl status prometheus
```