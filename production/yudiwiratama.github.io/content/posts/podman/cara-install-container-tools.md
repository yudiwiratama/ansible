---
title: "Cara Install Podman, Skopeo, dan Buildah Pada Linux (CentOS/Rocky)"
description: "Cara mudah install podman, skopeo, dan buildah pada linux"
date: "2021-08-10"
tags: ["Linux", "Container"]
categories: ["Podman"]
ShowToc: true
TocOpen: true
---

# Panduan
## Periksa versi yang tersedia untuk diinstall
```bash
sudo dnf module list container-tools
```
* Contoh keluaran pada Rocky Linux
```
[student@rocky ~]$ sudo dnf module list container-tools
Last metadata expiration check: 0:23:42 ago on Tue Aug 10 15:24:06 2021.
Rocky Linux 8 - AppStream
Name              Stream      Profiles     Summary
container-tools   rhel8 [d]   common [d]   Most recent (rolling) versions of podman, buildah, skopeo, runc, conmon, runc, conmon, CRIU, Udica, etc as well as dependencies such as container-selinux built and tested together, and updated as frequently as every 12 weeks.
container-tools   1.0         common [d]   Stable versions of podman 1.0, buildah 1.5, skopeo 0.1, runc, conmon, CRIU, Udica, etc as well as dependencies such as container-selinux built and tested together, and supported for 24 months.
container-tools   2.0         common [d]   Stable versions of podman 1.6, buildah 1.11, skopeo 0.1, runc, conmon, etc as well as dependencies such as container-selinux built and tested together, and supported as documented on the Application Stream lifecycle page.
container-tools   3.0         common [d]   Stable versions of podman 3.0, buildah 1.19, skopeo 1.2, runc, conmon, etc as well as dependencies such as container-selinux built and tested together, and supported as documented on the Application Stream lifecycle page.
```

## Install container-tools
```bash
sudo dnf module install container-tools:<stream>/<profile>
```

* Contoh install versi 3.0 dengan profil common
```bash
sudo dnf module install container-tools:3.0/common
```