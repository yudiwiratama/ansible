---
title: "Cara Import Container Image Containerd Kubernetes"
description: "Cara import container image containerd kubernetes"
date: "2021-10-24"
tags: ["Kubernetes", "Containerd"]
categories: ["Kubernetes"]
ShowToc: true
TocOpen: true
---

# Panduan
```bash
# Contoh:
# ctr -n=k8s.io images import <nama_file>
ctr -n=k8s.io images import nginx.tar
```

# Referensi
- [Scott's Blog](https://blog.scottlowe.org/2020/01/25/manually-loading-container-images-with-containerd/)