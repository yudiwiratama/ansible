---
title: "Cara Memasang OpenStack Exporter"
description: "Cara mudah memasang openstack exporter"
date: "2021-09-18"
tags: ["OpenStack", "Monitoring", "Exporter"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- OpenStack terinstall

# Panduan
1. Buat user `openstack-exporter`
```bash
useradd --no-create-home --shell /bin/false openstack-exporter
```

2. Buat file konfigurasi
```bash
mkdir /etc/openstack/
cat<<EOF > /etc/openstack/clouds.yaml
clouds:
 default:
   region_name: <region_name>
   identity_api_version: 3
   identity_interface: <endpoint_interface> # admin/internal/public
   auth:
     username: <username>
     password: <password>
     project_name: <project_name>
     project_domain_name: <project_domain_name>
     project_domain_id: <project_domain_id>
     user_domain_name: <user_domain_name>
     auth_url: https://<keystone_endpoint>:<keystone_port>/v3
     verify: false
EOF
```

3. Buat file systemd
```bash
cat<<EOF > /etc/systemd/system/openstack-exporter.service
[Unit]
Description=OpenStack Exporter OPEX
Wants=network-online.target
After=network-online.target

[Service]
User=openstack-exporter
Group=openstack-exporter
Type=simple
ExecStart=openstack-exporter Surabaya

[Install]
WantedBy=multi-user.target
EOF
```

4. Reload daemon dan jalankan servis
```bash
systemctl daemon-reload
systemctl enable --now openstack-exporter
```

> **Catatan!**  
> Jika terjadi error `x509: certificate signed by unknown` tambahkan certificate openstack ke daftar trusted!

# Referensi
- [Github - OpenStack Exporter](https://github.com/openstack-exporter/openstack-exporter)