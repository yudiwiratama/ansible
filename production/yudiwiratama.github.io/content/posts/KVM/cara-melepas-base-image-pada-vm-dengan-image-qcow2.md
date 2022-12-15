---
title: "Cara Melepas Base Image Pada VM dengan Image QCOW2"
description: "Cara melepas base image atau melakukan flatten pada VM dengan image QCOW2"
date: "2022-06-23"
tags: ["Linux", "KVM"]
categories: ["KVM"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Periksa base image
```bash
qemu-img info /data/vms/example.qcow2
```

Contoh
```bash
image: /data/vms/example.qcow2
file format: qcow2
virtual size: 50 GiB (53687091200 bytes)
disk size: 324 KiB
cluster_size: 65536
backing file: /data/isos/template-ubuntu20.img
backing file format: qcow2
Format specific information:
    compat: 0.10
    refcount bits: 16
```

2. Periksa target image VM
```bash
# virsh domblklist <nama_vm/domain>
virsh domblklist example
```

Contoh
```bash
 Target   Source
---------------------------------------------
 vda      example.qcow2
 hdd      /data/vms/sd-runner-cloudinit.iso
```

3. Pastikan VM dalam kondisi menyala dan mulai flatten image
```bash
# virsh blockpull <nama_vm/domain> vda --wait
virsh blockpull example vda --wait
```

4. Verifikasi
```bash
qemu-img info /data/vms/example.qcow2
```

Contoh
```bash
image: /data/vms/example.qcow2
file format: qcow2
virtual size: 50 GiB (53687091200 bytes)
disk size: 3.15 GiB
cluster_size: 65536
Format specific information:
    compat: 0.10
    refcount bits: 16
```

# Referensi
- [qemu-img cheatsheet](https://blog.programster.org/qemu-img-cheatsheet)