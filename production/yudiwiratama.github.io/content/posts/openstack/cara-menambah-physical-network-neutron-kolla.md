---
title: "Cara Menambahkan Physical Network Neutron (kolla-ansible)"
description: "Cara menambahkan physical network neutron (kolla-ansible)"
date: "2022-02-15"
tags: ["OpenStack", "Neutron", "Network"]
categories: ["OpenStack"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- ansible
- kolla-ansible

# Panduan
1. Tambahkan baris berikut pada konfigurasi `/etc/kolla/globals.yml`
```yaml
neutron_external_interface: "ens4,ens12,ens13"
neutron_bridge_name: "br-ex,br-ex2,br-ex3"
```

2. Rekonfigurasi servis neutron yang sudah berjalan
```bash
kolla-ansible -i ~/multinode reconfigure -t openvswitch,neutron
```

3. Periksa konfigurasi pada node yang menjalankan container neutron
```bash
grep bridge_mappings /etc/kolla/neutron-openvswitch-agent/openvswitch_agent.ini 
```

Contoh output
```bash
root@aa-hci01:~# grep bridge_mappings /etc/kolla/neutron-openvswitch-agent/openvswitch_agent.ini
bridge_mappings = physnet1:br-ex,physnet2:br-ex2,physnet3:br-ex3
```

# Referensi
- [kolla-ansible Docs - Networking](https://docs.openstack.org/kolla-ansible/latest/reference/networking/neutron.html)