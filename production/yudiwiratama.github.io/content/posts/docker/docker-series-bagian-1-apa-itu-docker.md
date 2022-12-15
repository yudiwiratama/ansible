---
title: "Docker Series Bagian 1 - Apa Itu Docker?"
description: "Mengenal docker"
date: "2022-05-19"
tags: ["Linux", "Container"]
categories: ["Docker"]
ShowToc: true
TocOpen: true
---

# Apa itu Docker?
Docker adalah platform terbuka yang digunakan untuk mengembangkan, mengemas, dan menjalankan aplikasi

# Teknologi dibalik docker
Docker ditulis menggunakan bahasa pemrograman `go` dan memanfaatkan fitur [namespace](https://en.wikipedia.org/wiki/Linux_namespaces) untuk mengisolasi proses yang disebut sebagai `container`

# Standar dalam *container*
[Open Container Initiatives (OCI)](https://opencontainers.org/) adalah yayasan yang dibangun untuk membuat standar atau struktur yang berkaitan dengan container runtime dan container image format

## Container Runtime (runtime-spec)
perangkat lunak yang bertanggung jawab untuk menjalankan container

## Container Image Format (image-spec)
Standar yang digunakan untuk mengatur bagaimana seharusnya container image dikemas yang bertujuan untuk memudahkan membuat, membawa, dan menyiapkan container image agar dapat dijalankan pada container runtime atau container engine apapun


# Referensi
- [Docker Overview](https://docs.docker.com/get-started/overview/)
- [Container Runtime](https://opensource.com/article/21/9/container-runtimes)
- [OCI runtime-spec](https://github.com/opencontainers/runtime-spec/blob/main/spec.md)
- [OCI image-spec](https://github.com/opencontainers/image-spec/blob/main/spec.md)