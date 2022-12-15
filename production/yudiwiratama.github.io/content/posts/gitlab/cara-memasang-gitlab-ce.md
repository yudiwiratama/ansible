---
title: "Cara Pasang GitLab CE [CentOS 8]"
description: "Cara mudah pasang GitLab Community Edition pada sistem operasi CentOS 8"
date: "2021-09-18"
tags: ["Linux", "Container"]
categories: ["Docker"]
ShowToc: true
TocOpen: true
---


# Panduan
1. Jalankan script untuk pemasangan repository
```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
```

2. Pasang GitLab

    - Menggunakan alamat Domain
    ```bash
    sudo EXTERNAL_URL="https://gitlab.example.com" dnf install -y gitlab-ce
    ```

    - Menggunakan alamat IP
    ```bash
    # dengan alamat IP
    sudo EXTERNAL_URL="https://192.168.10.10" dnf install -y gitlab-ce
    ```

3. Dapatkan password untuk login pertama kali
```bash
sudo cat /etc/gitlab/initial_root_password
```

4. Masuk GitLab via browser
![](/images/gitlab-ce-1.png)
![](/images/gitlab-ce-2.png)
