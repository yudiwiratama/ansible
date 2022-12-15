---
title: "Cara SSH Jump Host"
description: "Cara mudah melakukan ssh jump Host"
date: "2021-09-11"
tags: ["Linux", "SSH"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

![](/images/ssh-jump.png)

# Panduan
## Perintah
```bash
# 1 Lompatan
ssh -J <user>@<machine-1> <user>@<machine-2>

# 2 Lompatan
ssh -J <user>@<machine-1>,<machine-2> <user>@<machine-3>

# 3 Lompatan
ssh -J <user>@<machine-1>,<machine-2>,<user>@<machine-3> <user>@<machine-4>
```

## Contoh
### Contoh 1: SSH Jump dari 192.168.10.2 ke 192.168.10.3

![](/images/ssh-jump-1.png)

```bash
ssh -J user@192.168.10.2 user@192.168.10.3
```

### Contoh 2: SSH Jump dari 192.168.10.2 lalu 192.168.10.3 ke 192.168.10.4 

![](/images/ssh-jump-2.png)

```bash
ssh -J user@192.168.10.2,user@192.168.10.3 user@192.168.10.4
```

# Referensi
- [Gentoo Linux - SSH Jump Host](https://wiki.gentoo.org/wiki/SSH_jump_host)