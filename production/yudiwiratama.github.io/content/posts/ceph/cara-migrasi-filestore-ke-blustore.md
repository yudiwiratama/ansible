---
title: "Ceph - Cara Migrasi OSD Filestore ke Bluestore"
description: "Cara mudah migrasi OSD filestore ke bluestore"
date: "2021-12-07"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Periksa backend OSD Filestore atau Bluestore (Node MON)
```bash
ceph osd metadata <osd_id> | grep osd_objectstore
```

Periksa jumlah filestore dan bluestore yang ada pada klaster (Node MON)
```bash
ceph osd count-metadata osd_objectstore
```

2. Set out pada OSD yang ingin dimigrasi (Node MON)
```bash
ceph osd out <osd_id>
```


3. Tunggu sampai migrasi data selesai (Node MON)
```bash
while ! ceph osd safe-to-destroy $ID ; do sleep 60 ; done
```

4. Matikan service ceph-osd (Node OSD)

> Note: Ingat osd_id
```bash
sudo systemctl stop ceph-osd@<osd_id>.service
```

5. Hancurkan OSD
```bash
ceph-volume lvm zap --destroy /dev/<nama_device>
```

6. Informasikan pada klaster bahwa OSD telah dihancurkan
```bash
ceph osd destroy <osd_id> --yes-i-really-mean-it
```

7. Buat kembali OSD
```bash
ceph-volume lvm create --bluestore --data /dev/<nama_device> --osd-id <osd_id_sebelumnya>
```

# Referensi
- [Ceph Docs - Bluestore Migration](https://docs.ceph.com/en/latest/rados/operations/bluestore-migration/)