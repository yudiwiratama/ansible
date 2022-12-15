---
title: "Cara Memperbaiki OSD Tidak Bisa Up Pasca Upgrade Dari Luminous"
description: "cara memperbaiki osd tidak bisa up pasca upgrade ceph dari versi luminous"
date: "2021-11-06"
tags: ["Ceph", "Storage"]
categories: ["Ceph"]
ShowToc: true
TocOpen: true
---

![](/images/ceph-osd-unable-up-1.png)

![](/images/ceph-osd-unable-up-2.png)

```txt
osd.X tick checking mon for new map
```

# Penjelasan
Error ini muncul ketika melakukan upgrade ceph luminous > mimic, mimic > octopus. OSD tidak bisa up setelah direstart

# Panduan
1. Atur versi minimum sebuah osd dapat berpartisipasi pada klaster
```bash
# cek nilai sekarang
sudo ceph osd dump | grep require_osd

# lakukan perubahan
sudo ceph osd require-osd-release mimic
```

Penjelasan subcommand
```
osd require-osd-release luminous|mimic|nautilus|octopus [--yes-i-really-mean-it]                                      set the minimum allowed OSD release to participate in the cluster
```

2. Restart kembali OSD yang tidak bisa up
```bash
# seluruh osd pada host
sudo systemctl restart ceph-osd.target

# spesifik osd
sudo systemctl restart ceph-osd@<osd_id>
```

3. Hasil

![](/images/ceph-osd-unable-up-3.png)


# Referensi
- [Ceph Mailing List](https://lists.ceph.io/hyperkitty/list/ceph-users@ceph.io/message/RHJREQXM5N6KWDEEMLDVW44PYEILLR6E/)