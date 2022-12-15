---
title: "Cara Menyiapkan ISCSI Initiator (Linux)"
description: "Cara mudah mengkonsumsi LUN yang disediakan oleh iSCSI Target"
date: "2021-09-04"
tags: ["Linux", "ISCSI", "Storage"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Panduan
1. Install _package_ open-iscsi
- Debian Based
```bash
sudo apt install -y open-iscsi
```

- RHEL Based
```bash
sudo dnf install -y iscsi-initiator-utils 
# atau 
sudo yum install -y iscsi-initiator-utils 
```

2. Konfigurasi IQN initiator
```bash
sudo vim /etc/iscsi/initiatorname.iscsi
```

- Tambahkan baris berikut
    ```bash
    InitiatorName=iqn.2021-08.aa-lio-initiator:lio.initiator01
    ```

3. Konfigurasi iscsi initiator
```bash
sudo vim /etc/iscsi/iscsid.conf
```

- Sesuaikan baris berikut
    ```bash
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <user>
    node.session.auth.password = <password>
    ```

4. Restart servis iscsi
- Debian Based
```bash
sudo systemctl restart iscsid open-iscsi
```

- RHEL Based
```bash
sudo systemctl restart iscsid
```

5. _Discover_ target
```bash
sudo iscsiadm -m discovery -t sendtargets -p <ip_iscsi_target>
```

6. Masuk iSCSI target (Login)
```bash
sudo iscsiadm -m node --login
```

7. Verifikasi
```bash
sudo iscsiadm -m session -o show
```

# Referensi
- [Server World - iSCSI Initiator (Debian 11)](https://www.server-world.info/en/note?os=Debian_11&p=iscsi&f=3)
- [Server World - iSCSI Initiator (CentOS 8)](https://www.server-world.info/en/note?os=CentOS_8&p=iscsi&f=2)