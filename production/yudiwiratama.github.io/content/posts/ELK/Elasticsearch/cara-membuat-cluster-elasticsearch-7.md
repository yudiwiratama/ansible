---
title: "Cara Membuat Cluster Elasticsearch 7 beserta module security xpack [Debian/Ubuntu]"
description: "Cara mudah membuat cluster elasticsearch"
date: "2021-08-20"
tags: ["ELK", "Elasticsearch"]
categories: ["ELK"]
ShowToc: true
TocOpen: true
---

![](/images/elasticsearch-cluster.png)

# Panduan
1. Tambahkan host pada file /etc/hosts
```bash
vim /etc/hosts
```

* /etc/hosts
```
127.0.0.1 localhost

192.168.10.71 elasticsearch-node01
192.168.10.72 elasticsearch-node02
192.168.10.73 elasticsearch-node03
```

2. Unduh dan install package elasticsearch 
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.14.0-amd64.deb
dpkg -i elasticsearch-7.14.0-amd64.deb
```

3. Konfigurasi elasticsearch

* /etc/elasticsearch/elasticsearch.yml
```bash
cluster.name: elasticsearch-demo
node.name: elasticsearch-node01
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["elasticsearch-node01", "elasticsearch-node02", "elasticsearch-node03"]
node.master: true
node.data: true
discovery.zen.ping.unicast.hosts: ["192.168.10.71" , "192.168.10.72", "192.168.10.73"]
discovery.zen.minimum_master_nodes: 2
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate
xpack.security.transport.ssl.keystore.path: /etc/elasticsearch/elastic-certificates.p12
xpack.security.transport.ssl.truststore.path: /etc/elasticsearch/elastic-certificates.p12
```

4. Buat sertifikat SSL dari salah satu node. Sebagai contoh dari `elasticsearch-node01`
```bash
/usr/share/elasticsearch/bin/elasticsearch-certutil ca
/usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12

mv /usr/share/elasticsearch/elastic-certificates.p12 /etc/elasticsearch/elastic-certificates.p12
chown elasticsearch:elasticsearch /etc/elasticsearch/elastic-certificates.p12
```

5. Distribusikan sertifikat ke semua node
```bash
scp /etc/elasticsearch/elastic-certificates.p12 \
  elasticsearch-node02:/etc/elasticsearch/elastic-certificates.p12
scp /etc/elasticsearch/elastic-certificates.p12 \
  elasticsearch-node03:/etc/elasticsearch/elastic-certificates.p12

ssh elasticsearch-node02 chown elasticsearch:elasticsearch /etc/elasticsearch/elastic-certificates.p12
ssh elasticsearch-node03 chown elasticsearch:elasticsearch /etc/elasticsearch/elastic-certificates.p12
```

6. Jalankan dan enable service elasticsearch
```bash
systemctl enable --now elasticsearch
```

7. Setup password elasticsearch dari salah satu node
```bash
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto
```
> **Catatan**  
> Jangan lupa menyimpan password!

8. Verifikasi
```bash
curl -u elastic:$ELASTIC_PASSWORD http://192.168.10.71:9200/_cluster/health?pretty
```