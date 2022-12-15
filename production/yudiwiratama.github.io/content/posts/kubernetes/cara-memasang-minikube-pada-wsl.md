---
title: "Cara Memasang Minikube Pada WSL [Docker Driver]"
description: "Cara memasang minikube pada WSL menggunakan driver docker"
date: "2021-08-29"
tags: ["Kubernetes", "Minikube"]
categories: ["Kubernetes"]
ShowToc: true
TocOpen: true
---
![](/images/minikube-wsl2-docker.png)

# Prasyarat
- Windows 10
- WSL 2
- Docker Desktop WSL 2 Backend

# Panduan
1. Unduh dan install binary file minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

2. Jalankan minikube menggunakan driver docker
```bash
minikube start --driver=docker
```

3. Unduh dan install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

4. Verifikasi status minikube
```bash
minikube status

kubectl get pods -n kube-system
```

# Referensi
- [Docker Desktop WSL 2 backend](https://docs.docker.com/desktop/windows/wsl/)
- [Minikube Docker Driver](https://minikube.sigs.k8s.io/docs/drivers/docker/)
- [Install Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)