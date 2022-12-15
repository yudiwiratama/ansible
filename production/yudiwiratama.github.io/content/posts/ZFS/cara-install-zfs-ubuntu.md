---
title: "Cara Install ZFS Pada Linux [Ubuntu]"
description: "Cara mudah install ZFS pada Ubuntu"
date: "2021-08-09"
tags: ["Linux", "Filesystem", "ZFS"]
categories: ["ZFS"]
ShowToc: true
TocOpen: true
---

# Ubuntu
## Install package ZFS
```
sudo apt update
sudo apt install zfsutils-linux
```

## Verifikasi versi
```bash
modinfo zfs | grep version
```

# Referensi
* [OpenZFS Docs](https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/index.html)