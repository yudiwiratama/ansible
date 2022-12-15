---
title: "Upgrade Ceph Cluster (ceph-deploy)"
description: "Upgrade ceph cluster ceph-deploy"
date: "2021-11-09"
tags: ["Ceph", "Storage", "Upgrade"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- Ubuntu 18.04.06 LTS (Bionic Beaver)
- Ubuntu Cloud Archive Repository
- Ceph 13.2.9 - mimic (Sebelum upgrade)
- Ceph 15.2.14 - octopus (Setelah upgrade)

**Mesin**
| hostname         | ip address     |
|------------------|----------------|
| aa-bionic-ceph01 | 192.168.10.211 |
| aa-bionic-ceph02 | 192.168.10.212 |
| aa-bionic-ceph03 | 192.168.10.213 |

# Panduan
1. Periksa versi ceph pada klaster
```bash
sudo ceph versions
```

- keluaran
```json
{
    "mon": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 3
    },
    "mgr": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 3
    },
    "osd": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 9
    },
    "mds": {},
    "overall": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 15
    }
}
```

2. Pasang repository UCA ussuri pada semua host
```bash
sudo add-apt-repository cloud-archive:ussuri
```

3. Upgrade package ceph
```bash
# berpindah ke direktori ceph deploy
cd ~/ceph-deploy

# upgrade package pada mesin-mesin
ceph-deploy install aa-bionic-ceph01 aa-bionic-ceph02 aa-bionic-ceph03
```

4. Restart servis ceph-mon & ceph-mgr secara bergantian per mesin
```bash
# aa-bionic-ceph01
sudo systemctl restart ceph-mon@$(hostname) ceph-mgr@$(hostname)

# aa-bionic-ceph02
sudo systemctl restart ceph-mon@$(hostname) ceph-mgr@$(hostname)

# aa-bionic-ceph03
sudo systemctl restart ceph-mon@$(hostname) ceph-mgr@$(hostname)
```

5. Periksa versi ceph pada klaster
```bash
sudo ceph versions
```

- keluaran
```json
{
    "mon": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 3
    },
    "mgr": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 3
    },
    "osd": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 9
    },
    "mds": {},
    "overall": {
        "ceph version 13.2.9 (58a2a9b31fd08d8bb3089fce0e312331502ff945) mimic (stable)": 9,
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 6
    }
}
```

6. Restart servis ceph-osd secara bergantian per osd
```bash
# pasang flag noout pada klaster
ceph osd set noout

# restart semua ceph-osd pada host
sudo systemctl restart ceph-osd@<osd_id>

# Sebelum melanjutkan ke host berikutnya
# pastikan osd telah up kembali
ceph -s
ceph osd tree
```

7. Periksa versi ceph pada klaster
```bash
sudo ceph versions
```

- keluaran
```json
{
    "mon": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 3
    },
    "mgr": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 3
    },
    "osd": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 9
    },
    "mds": {},
    "overall": {
        "ceph version 15.2.14 (cd3bb7e87a2f62c1b862ff3fd8b1eec13391a5be) octopus (stable)": 15
    }
}
```

# Referensi
- [Ceph Docs - Upgrade Instructions](https://docs.ceph.com/en/latest/releases/octopus/#instructions)