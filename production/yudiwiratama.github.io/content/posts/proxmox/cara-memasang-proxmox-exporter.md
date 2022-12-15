---
title: "Cara Memasang Proxmox Exporter"
description: "Cara mudah memasang Proxmox Exporter"
date: "2021-09-11"
tags: ["Virtual Environment", "Proxmox", "Virtualisasi", "Monitoring"]
categories: ["Proxmox"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Proxmox terinstall

# Panduan
1. Buat token untuk proxmox-expoter
    - Buat API Token
        1. Datacenter > API Token
        2. Klik Add
        3. Masukan Token ID (Catat bagian ini)
        4. Hapus centang pada Privilege Separation
        5. Klik Add
    ![](/images/proxmox-exporter-1.png)
    - Salin Token Secret
        1. Klik "Copy Secret Value" (Catat bagian ini)
    ![](/images/proxmox-exporter-2.png)

2. Pasang `proxmox-exporter`
```bash
python3 -m pip install prometheus-pve-exporter
```

3. Buat servis systemd
    1. Buat user `pve_exporter`
    ```bash
    useradd --no-create-home --shell /bin/false pve_exporter
    ```

    2. Buat file konfigurasi
    ```bash
    mkdir /etc/pve_exporter
    cat<<EOF > /etc/pve_exporter/config.yaml
    default:
      user: root@pam
      token_name: exporter                               # Token ID (Langkah 1 > Buat API Token)
      token_value: 01144d44-487a-4ba0-b41d-fdba1d705ae0  # Secret Token (Langkah 1 > Salin Token Secret)
      verify_ssl: false    
    EOF
    ```

    3. Buat file environment
    ```bash
    cat<<EOF > /etc/default/pve_exporter
    CONFIG_FILE=/etc/pve_exporter/config.yaml
    LISTEN_ADDR=192.168.10.254
    LISTEN_PORT=9221
    EOF
    ```

    4. Buat file systemd
    ```bash
    cat<<EOF > /etc/systemd/system/pve_exporter.service
    [Unit]
    Description=PVE Expoter
    Wants=network-online.target
    After=network-online.target
    
    [Service]
    User=pve_exporter
    Group=pve_exporter
    Type=simple
    EnvironmentFile=/etc/default/pve_exporter
    ExecStart=pve_exporter \$CONFIG_FILE \$LISTEN_PORT \$LISTEN_ADDR 
    
    [Install]
    WantedBy=multi-user.target
    EOF
    ```

    5. Reload daemon dan jalankan servis
    ```bash
    systemctl daemon-reload
    systemctl enable --now pve_exporter
    ```

4. Verifikasi dengan membuka `http://<ip_exporter>:9221/pve?target=<ip_node_pve>`
![](/images/proxmox-exporter-3.png)

> **Catatan!**  
> Menambahkan  job pada konfigurasi prometheus
```yaml
  - job_name: 'pve_exporter'
    static_configs:
      - targets:
        - 192.168.10.86  # Proxmox VE node.
        - 192.168.10.87  # Proxmox VE node.
        - 192.168.10.88  # Proxmox VE node.
    metrics_path: /pve
    params:
      module: [default]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
```

# Referensi
- [Github - Proxmox Exporter]()