---
title: "Cara Menyiapkan Ceph Exporter [Containerized]"
description: "Cara mudah menyiapkan ceph exporter" 
date: "2021-09-18"
tags: ["Ceph", "Storage", "Monitoring", "Exporter"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- [Docker](https://blog.ajiarya.id/posts/docker/cara-pasang-docker-pada-linux/)
- Git

# Panduan
1. Clone repository ceph_exporter
```bash
git clone https://github.com/digitalocean/ceph_exporter.git
```

2. Buat container image
```bash
cd ceph_exporter/
docker build -t digitalocean/ceph_exporter .
```

3. Buat keyring untuk ceph_exporter

- Lakukan dari host yang memiliki ceph.conf dan ceph.client.admin.keyring untuk melakukan administrasi user
```bash
ceph auth add client.ceph_exporter mon 'allow r' mgr 'allow r' osd 'allow r' mds 'allow r'
ceph auth get client.ceph_exporter -o /etc/ceph/ceph.client.ceph_exporter.keyring

# Salin keyring ke monitoring node
scp /etc/ceph/{ceph.conf,ceph.client.ceph_exporter.keyring} monitoring-node:/etc/ceph
```

4. Jalankan container
```bash
docker run -d --name ceph_exporter -v /etc/ceph:/etc/ceph -e CEPH_USER=ceph_exporter -p=9128:9128 -it digitalocean/ceph_exporter
```

5. Akses endpoint `<IP_OR_DOMAIN>:9128`

![](/images/ceph-exporter.png)

> **Catatan!**  
> Menambahkan  job pada konfigurasi prometheus
```yaml
  - job_name: 'ceph_exporter'
    static_configs:
    - targets:
      - localhost:9128
```

# Referensi
- [Ceph Exporter Repository](https://github.com/digitalocean/ceph_exporter)