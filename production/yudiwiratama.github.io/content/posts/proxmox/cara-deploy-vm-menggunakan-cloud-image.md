---
title: "Cara Membuat VM Menggunakan Cloud Image - Proxmox"
description: "Cara mudah membuat VM dengan memanfaatkan cloud image"
date: "2021-09-10"
tags: ["Virtual Environment", "Proxmox", "Virtualisasi"]
categories: ["Proxmox"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Proxmox terinstall

# Panduan

## Buat template
1. Unduh file image cloud ubuntu 18
```bash
wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
```

2. Buat VM
```bash
qm create 100 --memory 2048 --net0 virtio,bridge=vmbr0
```

3. Import image ke dalam local-lvm storage
```bash
qm importdisk 100 bionic-server-cloudimg-amd64.img local-lvm
```

4. Atur disk VM
```bash
qm set 100 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-100-disk-0
```

5. Buat drive cloud-image
```bash
qm set 100 --ide2 local-lvm:cloudinit
```

6. Atur boot menggunakan cloud-init
```bash
qm set 100 --boot c --bootdisk scsi0
```

7. Atur console
```bash
qm set 100 --serial0 socket --vga serial0
```

8. Atur console
```bash
qm template 100
```

## Buat VM
1. Klon template 
```bash
qm clone 100 200 --name ubuntu20
```

2. Konfigurasi publickey dan IP Address
```bash
qm set 200 --sshkey ~/.ssh/id_rsa.pub
qm set 200 --ipconfig0 ip=192.168.10.251/24,gw=192.168.10.1
```

3. _Start_ VM
![](/images/proxmox-cloud-image-1.png)

4. Masuk _console_ VM (Cek proses booting)
![](/images/proxmox-cloud-image-2.png)

5. Uji SSH VM 
![](/images/proxmox-cloud-image-3.png)

# Referensi
- [Proxmox - Cloud Init](https://pve.proxmox.com/wiki/Cloud-Init_Support)