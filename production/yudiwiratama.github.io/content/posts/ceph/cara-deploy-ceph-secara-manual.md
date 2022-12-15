---
title: "Cara Deploy Ceph Quincy Secara Manual"
description: "Cara menyiapkan klaster ceph rilis quincy secara manual pada Ubuntu 20.04 LTS Focal"
date: "2022-08-30"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- Ubuntu 20.04 LTS Focal Fossa
- Ceph 17.2.X Quincy

# Panduan
## Prasyarat (Semua Ceph Node)
1. Konfigurasi berkas hosts
```bash
cat<<EOF | sudo tee -a /etc/hosts
10.150.17.11 ceph01
10.150.17.12 ceph02
10.150.17.13 ceph03
EOF
```

2. Memasang repository Ubuntu Cloud Archive (UCA) Focal-Yoga
```bash
sudo add-apt-repository cloud-archive:yoga
```

3. Update, upgrade, dan pasang ceph
```bash
sudo apt update; sudo apt upgrade -y
sudo apt install -y ceph
```

## Menyiapkan Ceph Monitor (ceph-mon)
### Eksekusi tahap 1 - 8 hanya pada ceph01
1. Membuat berkas konfigurasi ceph
```bash
sudo apt install -y uuid
FSID=$(uuid)

cat<<EOF | sudo tee -a /etc/ceph/ceph.conf
[global]
fsid = ${FSID}
mon initial members = ceph01, ceph02, ceph03
mon host = 10.150.17.11, 10.150.17.12, 10.150.17.13
public network = 10.150.17.0/24
auth cluster required = cephx
auth service required = cephx
auth client required = cephx
EOF

# Distribusikan ceph.conf
scp /etc/ceph/ceph.conf ceph02:/tmp/ceph.conf
ssh ceph02 sudo -S mv /tmp/ceph.conf /etc/ceph/ceph.conf
scp /etc/ceph/ceph.conf ceph03:/tmp/ceph.conf
ssh ceph03 sudo -S mv /tmp/ceph.conf /etc/ceph/ceph.conf
```

2. Buat keyring ceph-mon
```bash
sudo ceph-authtool \
  --create-keyring /tmp/ceph.mon.keyring \
  --gen-key -n mon. --cap mon 'allow *'
```

3. Buat keyring admin
```bash
sudo ceph-authtool \
  --create-keyring /etc/ceph/ceph.client.admin.keyring \
  --gen-key -n client.admin \
  --cap mon 'allow *' \
  --cap osd 'allow *' \
  --cap mds 'allow *' \
  --cap mgr 'allow *'
```

4. Membuat keyring bootstrap-osd
```bash
sudo ceph-authtool \
  --create-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring \
  --gen-key -n client.bootstrap-osd \
  --cap mon 'profile bootstrap-osd' \
  --cap mgr 'allow r'
```

5. Menambahkan keyring yang sudah dibuat sebelumnya ke berkas ceph.mon.keyring
```bash
sudo ceph-authtool /tmp/ceph.mon.keyring \
  --import-keyring /etc/ceph/ceph.client.admin.keyring
sudo ceph-authtool /tmp/ceph.mon.keyring \
  --import-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring
```

6. Mengubah kepimilikan berkas ceph.mon.keyring
```bash
sudo chown ceph:ceph /tmp/ceph.mon.keyring
```

7. Buat monitor map
```bash
monmaptool --create \
  --add ceph01 10.150.17.11 \
  --fsid ${FSID} /tmp/monmap

monmaptool \
  --add ceph02 10.150.17.12 \
  --fsid ${FSID} /tmp/monmap

monmaptool \
  --add ceph03 10.150.17.13 \
  --fsid ${FSID} /tmp/monmap
```

8. Distribusikan client.admin, ceph-mon keyring, and monmap ke ceph02 & ceph03
```bash
# Salin admin keyring
sudo cp /etc/ceph/ceph.client.admin.keyring /tmp/ceph.client.admin.keyring
sudo chown $USER:$USER /tmp/{ceph.client.admin.keyring,ceph.mon.keyring,monmap}

# Sebarkan berkas keyring dan monmap ke node lain
scp /tmp/{ceph.client.admin.keyring,ceph.mon.keyring,monmap} ceph02:/tmp
scp /tmp/{ceph.client.admin.keyring,ceph.mon.keyring,monmap} ceph03:/tmp

# Pindahkan admin keyring ke direktori /etc/ceph
ssh ceph02 sudo -S mv /tmp/ceph.client.admin.keyring /etc/ceph/
ssh ceph03 sudo -S mv /tmp/ceph.client.admin.keyring /etc/ceph/
```

### Eksekusi tahap 9 - 12 pada semua node
9. Buat direktori untuk ceph-mon
```bash
sudo -u ceph mkdir /var/lib/ceph/mon/ceph-$(hostname)
```

10. Inisiasi ceph-mon
```bash
sudo chown ceph:ceph /tmp/ceph.mon.keyring
sudo -u ceph ceph-mon --mkfs -i $(hostname) --monmap /tmp/monmap --keyring /tmp/ceph.mon.keyring
```

11. Enable dan jalankan service ceph-mon
```bash
sudo systemctl enable --now ceph-mon@$(hostname)
sudo systemctl status ceph-mon@$(hostname)
```

12. Periksa status cluster
```bash
sudo ceph -s
```

## Menyiapkan Ceph Manager (ceph-mgr)
1. Membuat keyring untuk ceph-mgr
```bash
sudo mkdir -p /var/lib/ceph/mgr/ceph-$(hostname)
sudo ceph auth get-or-create mgr.$(hostname) mon 'allow profile mgr' osd 'allow *' mds 'allow *' \
  -o /var/lib/ceph/mgr/ceph-$(hostname)/keyring
```

2. Mengubah kepemilikikan direktori
```bash
sudo chown -R ceph:ceph /var/lib/ceph/mgr
```

3. Enable dan menjalankan service
```bash
sudo systemctl enable --now ceph-mgr@$(hostname)
sudo systemctl status ceph-mgr@$(hostname)
```

4. Periksa status cluster
```bash
sudo ceph -s
```

### Tahap 5 hanya perlu dilakukan disalah satu node
5. Mengaktifkan modul msgr2
```bash
ceph mon enable-msgr2
```

## Menyiapkan Ceph Object Storage Daemon (ceph-osd)
1. Menyiapkan keyring untuk ceph-osd
```bash
# Eksekusi dari ceph01
sudo cp /var/lib/ceph/bootstrap-osd/ceph.keyring /tmp/ceph.keyring
sudo chown $USER:$USER /tmp/ceph.keyring

scp /tmp/ceph.keyring ceph02:/tmp/ceph.keyring
scp /tmp/ceph.keyring ceph03:/tmp/ceph.keyring
ssh ceph02 sudo -S cp /tmp/ceph.keyring /var/lib/ceph/bootstrap-osd/ceph.keyring
ssh ceph03 sudo -S cp /tmp/ceph.keyring /var/lib/ceph/bootstrap-osd/ceph.keyring

sudo chown ceph:ceph /var/lib/ceph/bootstrap-osd/ceph.keyring

ssh ceph02 sudo chown ceph:ceph /var/lib/ceph/bootstrap-osd/ceph.keyring
ssh ceph03 sudo chown ceph:ceph /var/lib/ceph/bootstrap-osd/ceph.keyring
```

2. Buat ceph-osd
```bash
# Eksekusi ceph01
sudo ceph-volume lvm create --bluestore --data /dev/vdb
sudo ceph-volume lvm create --bluestore --data /dev/vdc
sudo ceph-volume lvm create --bluestore --data /dev/vdd

# Eksekusi ceph02
sudo ceph-volume lvm create --bluestore --data /dev/vdb
sudo ceph-volume lvm create --bluestore --data /dev/vdc
sudo ceph-volume lvm create --bluestore --data /dev/vdd

# Eksekusi ceph03
sudo ceph-volume lvm create --bluestore --data /dev/vdb
sudo ceph-volume lvm create --bluestore --data /dev/vdc
sudo ceph-volume lvm create --bluestore --data /dev/vdd
```

# Hasil
## Ceph status
![](/images/cara-deploy-ceph-secara-manual.png)

## Ceph version
![](/images/cara-deploy-ceph-secara-manual-2.png)

# Baca Juga
- [Cara menghilangkan error insecure global_id reclaim](/posts/ceph/cara-menghilangkan-insecure-global-id-reclaim/)

# Referensi
- [Ceph Docs - Installation (Manual)](https://docs.ceph.com/en/latest/install/index_manual/#install-manual)