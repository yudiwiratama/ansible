---
title: "Cara Local Port Forwarding SSH"
description: "Cara agar kita bisa membuka port pada remote host dari localhost"
date: "2021-08-11"
tags: ["Linux", "SSH"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Ketika kita ingin mengakses sebuah layanan pada remote host yang hanya listen pada address `127.0.0.1` kita tidak bisa menggunakan fitur `Dynamic Port Forwarding` sebagai alternatif kita bisa menggunakan fitur `Local Port Forwarding`

# Panduan
```bash
ssh -L [local_address_bind]:[local_port_bind]:[remote_address_bind]:[remote_port_bind] <remote_host>
```

# Contoh
`serverku` menjalankan web server yang hanya listen pada address `127.0.0.1:80` lalu saya ingin mencoba mengakses web server tersebut dari `laptopku` pada address `192.168.10.10:8080`

1. Jalankan web server menggunakan python pada `serverku`
```bash
# Perintah python
[root@serverku ~]# python3 -m http.server --bind 127.0.0.1 80

# Periksa port 
[root@serverku ~]# ss -plunt 
Netid  State   Recv-Q  Send-Q           Local Address:Port    Peer Address:Port
tcp    LISTEN  0       5                    127.0.0.1:80           0.0.0.0:*     users:(("python3",pid=1999160,fd=3))

# curl port
[root@serverku ~]# curl 127.0.0.1:80
<h1>Hello from serverku</h1>
```

2. Lakukan SSH dari `laptopku` ke `serverku`
```bash
laptopku :: ~ » ssh -L 192.168.10.10:8080:127.0.0.1:80 serverku
```

3. Periksa port yang listen pada `laptopku`
```bash
laptopku :: ~ » ss -plunt
Netid   State    Recv-Q   Send-Q       Local Address:Port        Peer Address:Port   Process
tcp     LISTEN   0        128          192.168.10.10:8080             0.0.0.0:*       users:(("ssh",pid=8675,fd=4))
```

4. Uji coba `curl` ke `192.168.10.10:8080`
```bash
laptopku :: ~ » curl 172.21.149.107:8080
<h1>Hello from serverku</h1>
```