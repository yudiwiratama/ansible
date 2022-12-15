---
title: "Cara Membuat Rootless Container Podman Bisa Dikelola Sebagai Service"
description: "Cara mudah membuat rootless container podman bisa dikelola sebagai service"
date: "2021-08-23"
tags: ["Linux", "Container"]
categories: ["Podman"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Podman

# Panduan
Sebagai contoh kita akan membuat container nginx pada `non-root` user bernama `student`
1. Buat direktori untuk menyimpan konten
```bash
mkdir website
echo '<h1>Halo Dunia!</h1>' > website/index.html
```

2. Buat container nginx
```bash
podman run -d --name websiteku -p 8080:80 -v ~/website:/usr/share/nginx/html:Z nginx
```

3. Verifikasi container
```bash
podman ps

curl localhost:8080
```

* Contoh output
```bash
[student@podman-host ~]$ podman ps
CONTAINER ID  IMAGE                           COMMAND               CREATED             STATUS                 PORTS                 NAMES
109ed419f29e  docker.io/library/nginx:latest  nginx -g daemon o...  About a minute ago  Up About a minute ago  0.0.0.0:8080->80/tcp  websiteku
[student@podman-host ~]$ curl localhost:8080
<h1>Halo Dunia!</h1>
```

4. Buat direktori untuk menyimpan service systemd container
```bash
mkdir -p ~/.config/systemd/user/
```

5. Generate service systemd
```bash
cd ~/.config/systemd/user/
podman generate systemd --name websiteku --files --new
```

6. Hentikan dan hapus container yang telah dibuat
```bash
podman stop websiteku
podman rm websiteku
```

7. Jalankan dan enable service container-websiteku
```bash
systemctl --user daemon-reload
systemctl --user enable --now container-websiteku
```

8. Aktifkan `linger` pada user `student`
```bash
# Jalankan sebagai user student
loginctl enable-linger 
```
> **Catatan!**  
> linger berfungsi agar systemd service user yang telah dienable dapat berjalan saat system boot