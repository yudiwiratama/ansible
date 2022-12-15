---
title: "Cara Menyiapkan Monitoring Proxmox Menggunakan InfluxDB dan Grafana"
description: "Cara mudah mempersiapkan monitoring proxmox menggunakan influxdb dan grafana"
date: "2021-09-09"
tags: ["Virtual Environment", "Proxmox", "Virtualisasi", "Monitoring"]
categories: ["Proxmox"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Proxmox Terinstall

# Panduan
1. Pasang `influxdb` dan `influxdb-client`
```bash
apt install influxdb influxdb-client
```

2. Buat user database dan database
```bash
influx
```

(Dalam shell influx) Buat user dan database
```bash
CREATE USER admin WITH PASSWORD 'keraktelor' WITH ALL PRIVILEGES
CREATE DATABASE monitoring
CREATE USER influx WITH PASSWORD 'piesusu'
GRANT ALL ON monitoring TO influx
exit
```

3. Konfigurasi influxdb
```bash
vi /etc/influxdb/influxdb.conf
```

Sesuaikan baris berikut
```bash
[[udp]]
   enabled = true
   bind-address = ":8089"
   database = "mondb"
```

4. Restart InfluxDB
```bash
systemctl restart influxdb 
```

5. Tambahkan Metric Server
    - Tambahkan InfluxDB
    ![](/images/proxmox-ext-metric.png)

6. Pasang Grafana
    - Tambahkan apt key
    ```bash
    wget -q -O - https://packages.grafana.com/gpg.key | apt-key add -
    ```

    - Tambahkan repository grafana
    ```bash
    echo "deb https://packages.grafana.com/oss/deb stable main" | tee -a /etc/apt/sources.list.d/grafana.list
    ```

    - Jalankan apt update
    ```bash
    apt update
    ```

    - Pasang `grafana`
    ```bash
    apt install -y grafana
    ```

    - Jalankan dan _enable_ servis grafana
    ```bash
    systemctl daemon-reload
    systemctl enable --now grafana-server
    ```

7. Tambahkan InfluxDB sebagai datasource pada Grafana
    - Configuration > Data sources > Add data source
    - Lakukan konfigurasi seperti berikut:
        - Isikan HTTP > URL. Isikan URL: `http://<IP_Address>:8086`
        - Isikan InfluxDB Details > Databases. Isikan Database: `<database_name>` sesuai dengan langkah 2
        - Isikan InfluxDB Details > User. Isikan User: `<user_influx>` sesuai dengan langkah 2
    ![](/images/proxmox-influxdb.png)

8. (Opsional) Import grafana dashboard yang sudah tersedia pada grafana.com
    - https://grafana.com/grafana/dashboards/10048

    1. Create > Import
    ![](/images/proxmox-grafana-import.png)

    2. Masukan ID dashboard
    ![](/images/proxmox-grafana-import-2.png)

    3. Pilih sumber InfluxDB yang telah ditambahkan dari tahap 6
    ![](/images/proxmox-grafana-import-3.png)

## Hasil
![](/images/proxmox-grafana-dashboard.png)