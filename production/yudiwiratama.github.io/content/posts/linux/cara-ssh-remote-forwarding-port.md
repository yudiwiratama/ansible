---
title: "Cara Remote Port Forwarding SSH"
description: "Cara agar kita bisa mengakses port pada localhost dari remote host"
date: "2021-08-23"
tags: ["Linux", "SSH"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Remote Port Forwarding adalah salah satu cara agar kita bisa menyediakan service atau port yang ada pada `localhost` agar bisa digunakan oleh `remote host`.

Sebagai contoh kita akan menggunakan `squid3` yang tersedia pada `localhost` yang kemudian akan diakses oleh `remote host`

# Panduan
```bash
ssh -R [remote_address_bind]:[remote_port_bind]:[local_address_bind]:[local_port_bind] <remote_host>
```

# Contoh
`laptopku` menjalankan proxy server yang tersedia pada alamat `0.0.0.0:3128` lalu saya ingin mencoba mengakses proxy server tersebut dari `home-lab` pada alamat `127.0.0.1:8080`

1. Proxy Server Port
```bash
laptopku :: ~ » ss -plunt
Netid                State                 Recv-Q                Send-Q                                Local Address:Port                                  Peer Address:Port                Process
tcp                  LISTEN                0                     4096                                              *:3128                                             *:*
```

2. Jalankan perintah ssh remote forward
```bash
ssh -R 8080:localhost:3128 student@home-lab
```

3. Verifikasi port

Pastikan ada port 8080 yang listen dengan proses `sshd`
```bash
student@home-lab:~$ sudo ss -plunt
Netid              State               Recv-Q              Send-Q                                     Local Address:Port                             Peer Address:Port              Process
tcp                LISTEN              0                   128                                            127.0.0.1:8080                                  0.0.0.0:*                  users:(("sshd",pid=1740,fd=11))
tcp                LISTEN              0                   128                                                [::1]:8080                                     [::]:*                  users:(("sshd",pid=1740,fd=10))
```

4. Coba gunakan proxy
```bash
export http_proxy=localhost:8080
export https_proxy=localhost:8080

curl -v google.com
```
![](/images/ssh-remote-port-forwarding.png)