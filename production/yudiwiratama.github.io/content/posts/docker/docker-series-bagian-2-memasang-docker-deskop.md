---
title: "Docker Series Bagian 2 - Memasang Docker Desktop"
description: "Memasang docker desktop untuk alat belajar"
date: "2022-06-08"
tags: ["Linux", "Container"]
categories: ["Docker"]
ShowToc: true
TocOpen: true
---

# Panduan Linux

**Panduan:** https://docs.docker.com/desktop/linux/install/

**Prasyarat Sistem:**
1. kernel 64-bit dan CPU mendukung virtualisasi
2. mendukung virtualisasi KVM
3. QEMU dengan versi 5.2 atau lebih baru
4. Sistem Operasi dengan systemd init
5. Setidaknya memiliki 4GB RAM

**Rangkuman tahap:**
1. Install qemu

    1.1. Debian/Ubuntu
    ```bash
    sudo apt install qemu-system-x86 libvirt-daemon-system libvirt-clients bridge-utils
    ```

    1.2. Red Hat/CentOS/Fedora
    ```bash
    # dnf
    sudo dnf groupinstall -y "Virtualization Host"
    
    # yum
    sudo yum groupinstall -y "Virtualization Host"
    ```
2. Aktifkan modul kernel KVM
```bash
# CPU AMD
sudo modprobe kvm_amd

# CPU Intel
sudo modprobe kvm_intel

# Verifikasi (Pastikan output tidak kosong)
lsmod | grep kvm
```

3. Tambahkan user ke group kvm
```bash
# tambahkan user ke group kvm
sudo usermod -aG kvm $USER

# logout
exit

# login kembali dan periksa permission user
# pastikan kvm masuk kedalam daftar group
groups
```

4. Download package sesuai dengan sistem operasi `.deb` atau `.rpm`

   [klik disini](https://docs.docker.com/desktop/linux/install/)
![](/images/docker-series-part-2-memasang-docker-deskop-1.png)

5. pasang package yang telah didownload
```
# GUI: Klik nama package dua kali

# Debian/Ubuntu
sudo apt install -y ./<nama package deb>

# Red Hat/CentOS/Fedora
sudo rpm -i <nama package rpm>
```

6. Jalankan aplikasi docker desktop dengan mencari Docker Desktop
![](https://docs.docker.com/desktop/linux/images/docker-app-in-apps.png)
Image source: https://docs.docker.com/desktop/windows/images/docker-app-search.png

# Panduan Windows
**Rekomendasi:** Jalankan Docker Desktop dengan backend WSL2

**Panduan:** https://docs.docker.com/desktop/windows/install/

**Prasyarat Sistem:**
1. Windows 10 64-bit atau 11 64-bit Home / Pro / Enterprise / Education [Informasi lengkapnya disini](https://docs.docker.com/desktop/windows/install/)

2. Pasang WSL [Panduan pasang WSL](https://www.windowscentral.com/how-install-wsl2-windows-10)

3. Aktifkan fitur WSL 2 Pada Distribusi linux pilihan [Baca lengkap disini](https://docs.microsoft.com/en-us/windows/wsl/install)
```
# Periksa versi WSL
wsl -l -v

# Mengubah WSL menjadi versi 2
wsl --set-version <name_distro> 2
```

**Rangkuman tahap:**

1. Download installer Docker Desktop untuk Windows

    [Klik disini](https://docs.docker.com/desktop/windows/install/)
![](/images/docker-series-part-2-memasang-docker-deskop-2.png)

2. Jalankan installer

3. Jalankan aplikasi docker desktop dengan mencari Docker Desktop
![](https://docs.docker.com/desktop/windows/images/docker-app-search.png)
Image source: https://docs.docker.com/desktop/windows/images/docker-app-search.png

# Panduan Mac

**Panduan:** https://docs.docker.com/desktop/mac/install/#install-interactively

**Prasyarat Sistem:**
- Intel Chip - AMD64
    1. MacOS dengan versi 10.15 atau lebih baru
    2. Setidaknya memiliki 4GB RAM
    3. Tidak boleh terinstall VirtualBox dengan versi dibawah 4.3.30 pada sistem

- Apple Chip - ARM64
    1. Install rosetta
    ```bash
    softwareupdate --install-rosetta
    ```

**Rangkuman tahap:**
1. Unduh disk image file

    - [klik disini untuk AMD64](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64)
    - [klik disini untuk ARM64](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64)

2. Klik dua kali pada disk image yang sudah didownload, seret dan jatuhkan pada folder applications
![](https://docs.docker.com/desktop/mac/images/docker-app-drag.png)
Image source: https://docs.docker.com/desktop/mac/images/docker-app-drag.png

3. Buka folder application dan cari docker
![](https://docs.docker.com/desktop/mac/images/docker-app-in-apps.png)
Image source: https://docs.docker.com/desktop/mac/images/docker-app-in-apps.png

4. Baca *Docker Subscription Service Agreement* dan setujui jika setuju

# Referensi
- [Install Docker Desktop on Linux](https://docs.docker.com/desktop/linux/install/)
- [Install Docker Desktop on Windows](https://docs.docker.com/desktop/windows/install/)
- [Install Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install/)