---
title: "Memilih Deployment Tool Ceph"
description: "Cara memilih deployment tool ceph"
date: "2021-09-05"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
## Rekomendasi
- [cephadm](https://docs.ceph.com/en/latest/cephadm/#cephadm) (Container)

    cephadm tersedia sejak versi v15.2.0 (Octopus). cephadm mendeploy servis-servis Ceph dalam bentuk container, dengan cephadm mempermudah untuk memanajemen klaster seperti menambahkan, menghapuskan, atau memperbarui komponen Ceph.

    cephadm mendukung docker dan podman sebagai container runtime.

- [Rook](https://github.com/rook/rook) (Container / Manage by Kubernetes)
    
    Rook tersedia sejak versi Nautilus. Rook men-deploy dan melakukan manajemen klaster Ceph yang berjalan didalam Kubernetes. Jika ingin menggunakan Ceph sebagai storage untuk klaster Kubernetes, Rook adalah tool cocok digunakan.

## Lainnya
- [ceph-ansible](https://docs.ceph.com/projects/ceph-ansible/en/latest/) (Service or Container)
    
    Deploy Ceph menggunakan Ansible, playbook yang sudah disediakan mempermudah kita untuk men-deploy klaster ceph. ceph-ansible mendukung deployment servis-servis Ceph sebagai _Systemd Service_ atau sebagai _Container_.
  
- [ceph-deploy (Deprecated!)](https://docs.ceph.com/projects/ceph-deploy/en/latest/) (Service)

    ceph-deploy merupakan tool yang memanfaatkan SSH dan python untuk melakukan deploy servis-service Ceph, penggunaannya sangat mudah namun tool ini sudah deprecated semenjak release Octopus.
   
- [Charmed Ceph (Juju)](https://ubuntu.com/ceph/docs/install) (Service and/or Container)

    Charmed Ceph adalah deployment yang sering digunakan ketika kita ingin menggunakan MAAS + Juju untuk manajemen server dan aplikasi.
    
    Charmed OpenStack + Charmed Ceph adalah kombinasi yang sering digunakan.