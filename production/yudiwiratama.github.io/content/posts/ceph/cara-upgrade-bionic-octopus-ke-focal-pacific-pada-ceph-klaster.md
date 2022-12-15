---
title: "Ceph - Cara Upgrade Dari Bionic Octopus ke Focal Pacific"
description: "Cara melakukan upgrade dari Ubuntu Bionic Ceph Octopus ke Ubuntu Focal Ceph Pacific"
date: "2021-11-06"
tags: ["Ceph", "Storage", "Upgrade"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Rencana Upgrade
**Dari**:
- Ubuntu 18.04 (Bionic)
- Ceph 15.2.X (Octopus)

**Ke**:
- Ubuntu 20.04 (Focal)
- Ceph 16.2.X (Pacific)

# Prasyarat
- Klaster dalam keadaan **HEALTH_OK**

# Panduan
1. Pasang flag pada klaster dan set default 
```bash
sudo ceph osd set noout
sudo ceph osd set nobackfill
sudo ceph osd set norecover
sudo ceph osd set norebalance
```

2. Update dan upgrade package
```bash
sudo apt update -y; sudo apt upgrade -y
```

3. Upgrade ke Ubuntu Focal
```bash
sudo apt full-upgrade
sudo do-release-upgrade -f DistUpgradeViewNonInteractive
```

4. Reboot mesin
```bash
sudo reboot
```

5. Upgrade package ceph
```bash
#1: semua package ceph
sudo apt install -y ceph

#2: spesifik package ceph
sudo apt install -y ceph-mon
sudo apt install -y ceph-mgr
sudo apt install -y ceph-osd
```

6. Restart servis ceph-mon dan ceph-mgr
```bash
sudo systemctl restart ceph-mon.target ceph-mgr.target
```

7. Periksa versi ceph pada klaster
```bash
sudo ceph versions
```

- keluaran
```json
{
    "mon": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 3
    },
    "mgr": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 3
    },
    "osd": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 9
    },
    "mds": {},
    "overall": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 9,
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 6
    }
}
```

8. Restart servis ceph-osd
```bash
sudo systemctl restart ceph-osd.target
```

9. Periksa versi ceph pada klaster
```bash
sudo ceph versions
```

- keluaran
```json
{
    "mon": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 3
    },
    "mgr": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 3
    },
    "osd": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 9
    },
    "mds": {},
    "overall": {
        "ceph version 16.2.6 (ee28fb57e47e9f88813e24bbf4c14496ca299d31) pacific (stable)": 15
    }
}
```

10. Lepas flag pada klaster
```bash
sudo ceph osd unset noout
sudo ceph osd unset nobackfill
sudo ceph osd unset norecover
sudo ceph osd unset norebalance
```

11. Hasil

![](/images/ceph-upgrade-bionic-octopus-to-focal-pacific.png)

# Referensi
- [Ubuntu Docs - Upgrade Introduction](https://ubuntu.com/server/docs/upgrade-introduction)
- [Ceph Docs - Upgrading Non-cephadm Clusters](https://docs.ceph.com/en/latest/releases/pacific/#upgrading-non-cephadm-clusters)