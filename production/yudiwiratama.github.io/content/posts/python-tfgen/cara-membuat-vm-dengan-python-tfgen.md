---
title: "Cara Membuat VM Libvirt Menggunakan python-tfgen"
description: "python-tfgen"
date: "2021-08-17"
tags: ["Terraform", "Linux", "Virtualisasi", "KVM", "python-tfgen"]
categories: ["Automation"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- [Terraform](/posts/linux/cara-menggunakan-terraform-libvirt-provider/)
- [python-tfgen](https://github.com/ajiarya/python-tfgen)
- Storage Pool ([Baca disini untuk cara membuat Storage Pool](/posts/python-tfgen/cara-membuat-pool-dengan-python-tfgen/))
- Image ([Baca disini untuk cara membuat Image](/posts/python-tfgen/cara-membuat-image-dengan-python-tfgen/))
- Network ([Baca disini untuk cara membuat Network](/posts/python-tfgen/cara-membuat-network-dengan-python-tfgen/))

# Panduan
1. Buat yaml dengan `kind: vm` lalu isikan dengan daftar image yang diinginkan
* image.yaml
```
kind: vm
uri: "qemu:///system"
spec:
  - name: vm1 
    hostname: vm1
    nested_enabled: true
    os: ubuntu
    vcpus: 2
    memory: 2G
    console: vnc
    cloud_data:
      users:
        - name: root
          public_key:
            - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBpZKhFbJ9r/gEef9KYo13VoEgBOkyMw06aOCzltT8wsjYPRMYzb2cbuiJBqJq5sXBrpaunTh5M7F9TyHQxhZnGgGVdutX7Q3RTOxWCHNMOxnrN7gQAHQ0kdqegrvUKNB7ym/2G3baz7pmxXf+I1Tw5AChJ8kIBDB9DnzVdtnMYxT0nivY1f6gSR2cgStEsuSbZDlBQ5Lt6W+sUyNkffddpZl0+QHAFM6UFTJNcuwMBqQG75/ZcwqLkQKU6pg0kZnDgmElXtJUKu141PJ0EbhkeOsh+zjYuwScotQYbN/MLN8fAhlQLFOwX/g3o6M9A49jyHVglQCxP2mI+d3Fra+ykrcQ7eTkPncwCCEmZYLjonQl3qhHHcbM7He1kOnORdVC/f/Uz4VsX+cMn5WGs1P+qyZKRAvw6egqWAkS0GPfEjdkhqLQ4WU9mC26aaaQfBqmMos1XZ/nRTuGAc6qb0SOkz5XN0g5ISNmCUpsnxO4YocmWP/sNDRPWBIzcycZE6s= student@home-lab
    base_image:
      storage_pool: images
      name: ubuntu-focal
    disks:
      storage_pool: vms
      disks:
        - name: vm1-vda.qcow2
          size: 10G
    networks:
      - name: home-lab-network
        address: 192.168.100.10/24
        mtu: 1500
        gateway: 192.168.10.1
        dns: [8.8.8.8, 8.8.4.4]
```

2. Buat HCL berdasarkan yaml yang telah dibuat
```bash
./tfgen.py -f vm.yaml -o vm
```

3. Berpindah ke direktori yang dihasilkan python-tfgen
```bash
cd vm/
```

4. Jalankan inisiasi terraform
```bash
terraform init
```

5. Jalankan terraform untuk membuat VM
```bash
terraform apply
```
> **Tips!**  
> Gunakan `terraform apply -auto-approve` untuk melewati prompt persetujuan.

6. Verifikasi VM yang telah terbuat
```bash
virsh list
```

7. SSH VM
```bash
ssh root@192.168.100.10
```

![](/images/python-tfgen-vm.jpg)

# Panduan Menghapus
```bash
terraform destroy
```