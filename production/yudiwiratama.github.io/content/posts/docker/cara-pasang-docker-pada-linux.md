---
title: "Cara Pasang Docker Pada Linux (CentOS/Ubuntu)"
description: "Cara mudah pasang container runtime docker pada sistem operasi linux"
date: "2021-09-13"
tags: ["Linux", "Container"]
categories: ["Docker"]
ShowToc: true
TocOpen: true
---

# Panduan
## CentOS
1. Pasang repository resmi docker
```bash
# yum
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# dnf
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

2. Pasang docker
```bash
# yum
sudo yum install -y docker-ce docker-ce-cli containerd.io

# dnf
sudo dnf install -y docker-ce docker-ce-cli containerd.io
```

3. Enable dan jalankan servis
```bash
systemctl enable --now docker
```

4. Verifikasi
```bash
docker info
```

## Ubuntu
1. Pasang repository resmi docker
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

2. Pasang docker
```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

3. Enable dan jalankan servis
```bash
systemctl enable --now docker
```

4. Verifikasi
```bash
docker info
```

# Referensi
- [Docker Docs](https://docs.docker.com/engine/install/)