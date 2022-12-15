---
title: "Cara Deploy Ceph Dashboard Pada Deployment Manual"
description: "Cara mudah deploy ceph dashboard pada deployment manual"
date: "2021-08-29"
tags: ["Ceph", "Storage", "Dashboard"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Klaster Ceph

# Panduan
1. Install package `ceph-mgr-dashboard` pada semua host `ceph-mgr`
    - Debian/Ubuntu
    ```bash
    sudo apt install -y ceph-mgr-dashboard
    ```

    - RHEL/CentOS/Rocky 8
    ```bash
    sudo dnf install -y ceph-mgr-dashboard
    ```

    - RHEL/CentOS/Rocky 7
    ```bash
    sudo yum install -y ceph-mgr-dashboard
    ```

2. Aktifkan module dashboard pada `ceph-mgr`
```bash
ceph mgr module enable dashboard
```

3. Buat TLS untuk ceph dashboard
```bash
ceph dashboard create-self-signed-cert
```

4. Buat user
```bash
echo "rahasia123" > password.txt
ceph dashboard ac-user-create admin -i password.txt administrator
```

5. Login ceph dashboard

- **URL:** https://<alamat_ceph-mgr>:8443

![](/images/ceph-dashboard-on-manual-deployment.png)