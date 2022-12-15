---
title: "Cara Memasang OpenShift CodeReady Pada Fedora 34"
description: "Cara mudah memasang OpenShift codeready pada Fedora 34"
date: "2021-08-29"
tags: ["OKD", "Kubernetes"]
categories: ["OKD"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Mesin Fedora 34 (Baremetal atau VM)
- Akun Red Hat

# Versi
- **Sistem Operasi:** Fedora 34 Cloud Image
- **CodeReady:** 1.31.2+19538dab
- **OpenShift:** 4.8.4

# Panduan
1. Update dan Upgrade package
```bash
sudo dnf update -y; sudo dnf upgrade -y
```

2. Install package `NetworkManager` dan `wget`
```bash
sudo dnf install -y NetworkManager wget
```

3. Unduh CodeReady dan salin pull secret [Tautan Disini](https://cloud.redhat.com/openshift/create/local)

![](/images/okd-download-and-pull-secret.png)

```bash
wget https://developers.redhat.com/content-gateway/file/pub/openshift-v4/clients/crc/1.31.2/crc-linux-amd64.tar.xz
tar xf crc-linux-amd64.tar.xz

cd crc-linux-1.31.2-amd64/
sudo mv crc /usr/local/bin

# Verifikasi
crc version
```

4. Setup dan jalankan CodeReady (Tempel pull secret pada proses `crc start`)
```bash
crc setup
crc start
```

![](/images/okd-codeready.png)

5. Akses CodeReady
    - Akses CLI
    ```bash
    # Jika ingin melihat kredensial jalankan perintah berikut:
    # crc console --url --credentials
    eval $(crc oc-env)
    oc login -u kubeadmin -p 84C7Q-C3Ecq-aWYXF-V9zR8 https://api.crc.testing:6443
    ```

    - Akses Browser

    1. Siapkan tunnel
    2. Akses https://console-openshift-console.apps-crc.testing
    ![](/images/okd-codeready-console.png)
    



# Referensi
- [Dokumentasi CodeReady](https://www.okd.io/crc.html)
