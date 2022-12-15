---
title: "Memasang Kata Container dengan Containerd Pada Ubuntu 20.04"
description: "Cara mudah memasang Kata Container dengan Containerd pada Sistem Operasi Ubuntu 20.04"
date: "2021-08-03"
tags: ["Container", "Containerd", "Kata", "OpenInfra"]
categories: ["Kata"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Baremetal atau Virtualisasi yang mendukung _Nested Virtualization_

# Panduan
## 1. Pasang Repository Kata Container
```
ARCH=$(arch)
BRANCH="${BRANCH:-master}"
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/home:/katacontainers:/releases:/${ARCH}:/${BRANCH}/xUbuntu_$(lsb_release -rs)/ /' > /etc/apt/sources.list.d/kata-containers.list"
curl -sL  http://download.opensuse.org/repositories/home:/katacontainers:/releases:/${ARCH}:/${BRANCH}/xUbuntu_$(lsb_release -rs)/Release.key | sudo apt-key add -
sudo -E apt-get update
sudo -E apt-get -y install kata-runtime kata-proxy kata-shim
```

## 2. Periksa kemampuan host untuk menjalankan Kata Container
```bash
kata-runtime kata-check
```

**Keluaran yang diharapkan**
```bash
student@kata-host:~$ kata-runtime kata-check
No newer release available
System is capable of running Kata Containers
```

## 3. Pasang Containerd
```bash
sudo apt-get -y install containerd
```

## 4. Coba jalankan sebuah container menggunakan Kata Container
```bash
sudo ctr run --runtime io.containerd.run.kata.v2 -t --rm docker.io/library/busybox:latest kata-demo sh
```

## 5. Periksa proses container
```bash
htop
```
![](/images/kata-process.jpg)
[Gambar Kata Process](/images/kata-process.jpg)


# Referensi
- https://github.com/kata-containers/documentation/blob/master/how-to/containerd-kata.md