---
title: "Cara Mengganti Password User Elasticsearch"
description: "Cara mudah mengganti password user Elasticsearch"
date: "2021-08-09"
tags: ["ELK", "Elasticsearch"]
categories: ["ELK"]
ShowToc: true
TocOpen: true
---

# Panduan

## Perintah mengganti password user sendiri
```bash
curl -u <user> -XPUT \
  'http://localhost:9200/_xpack/security/user/_password?pretty' \
  -H 'Content-Type: application/json' -d' 
  { "password": "<password>" }' 
```

### Contoh mengganti password user sendiri
```bash
curl -u elastic -XPUT \
  'http://localhost:9200/_xpack/security/user/_password?pretty' \
  -H 'Content-Type: application/json' -d' 
  { "password": "sangatrahasia" }' 
```

### Contoh output
```bash
[root@rocky ~]# curl -u elastic -XPUT \
>   'http://localhost:9200/_xpack/security/user/_password?pretty' \
>   -H 'Content-Type: application/json' -d'
> { "password": "sangatrahasia" }'
Enter host password for user 'elastic':
{ }
```

## Perintah mengganti password user lain (Admin)
```bash
curl -u <user_admin> -XPUT \
  'http://localhost:9200/_xpack/security/user/<user_lain>/_password?pretty' \
  -H 'Content-Type: application/json' -d' 
  { "password": "<password>" }' 
```

### Contoh mengganti password user lain (Admin)
```bash
curl -u elastic -XPUT \
  'http://localhost:9200/_xpack/security/user/kibana/_password?pretty' \
  -H 'Content-Type: application/json' -d' 
  { "password": "inipasswordkibana" }' 
```

### Contoh output
```bash
[root@rocky ~]# curl -u elastic -XPUT \
>   'http://localhost:9200/_xpack/security/user/kibana/_password?pretty' \
>   -H 'Content-Type: application/json' -d'
>   { "password": "inipasswordkibana" }'
Enter host password for user 'elastic':
{ }
```

Referensi
* [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-change-password.html#security-api-change-password-request)