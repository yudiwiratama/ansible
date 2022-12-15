---
title: "Cara Import & Export Docker Image"
description: "Cara import dan export docker image melalui Docker CLI"
date: "2021-10-24"
tags: ["Linux", "Container"]
categories: ["Docker"]
ShowToc: true
TocOpen: true
---

# Panduan
## Simpan docker image
- Tanpa kompresi
```bash
# Contoh:
# docker save name_image/id_image:tag > nama_output
docker save nginx:latest > nginx.tar
```

- Dengan kompresi
```bash
# Contoh:
# docker save name_image/id_image:tag | gzip > nama_output
docker save nginx:latest | gzip > nginx.tar.gz
```

Perbandingan tanpa kompresi dengan kompresi gzip terhadap container image nginx 
```bash
smolflow :: ~ » ls -lh nginx.tar nginx.tar.gz
-rw-r--r-- 1 ada ada 132M Oct 24 20:16 nginx.tar
-rw-r--r-- 1 ada ada  50M Oct 24 20:16 nginx.tar.gz
```

## Muat docker image
```bash
# Contoh:
# docker load < nama_file
docker load < nginx.tar
```

# Referensi
- [Docker Docs - Save](https://docs.docker.com/engine/reference/commandline/save/)
- [Docker Docs - Load](https://docs.docker.com/engine/reference/commandline/load/)