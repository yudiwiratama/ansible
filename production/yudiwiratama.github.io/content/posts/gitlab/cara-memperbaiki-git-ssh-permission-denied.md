---
title: "Cara Memperbaiki Git SSH Permission Denied GitLab [Red Hat/CentOS]"
description: "Cara memperbaiki git ssh permission denied GitLab pada sistem operasi Red Hat atau CentOS"
date: "2021-09-12"
tags: ["GitLab", "Git", "Troubleshoot"]
categories: ["GitLab"]
ShowToc: true
TocOpen: true
---

# Masalah
![](/images/gitlab-git-ssh-error-1.png)
```
git@gitlab.example.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
```
Tidak bisa git clone walaupun sudah menambahkan public key ssh pada akun GitLab

## Kondisi
1. Sudah menambahkan Public Key SSH pada akun GitLab
2. Tidak bisa menjalankan `git clone` ke repository private walaupun sebagai pemilik repository

# Panduan
1. Lihat error pada SELinux
```bash
journalctl -t setroubleshoot 
```
![](/images/gitlab-git-ssh-error-2.png)
Error terjadi karena file authorized_key

2. Lihat _SELinux Context_ pada direktori `.ssh` gitlab
```bash
ls -Z /var/opt/gitlab/.ssh/authorized_keys

# Output:
# unconfined_u:object_r:var_t:s0 /var/opt/gitlab/.ssh/authorized_keys
```

3. Ubah SELinux Context pada direktori tersebut
```bash
semanage fcontext --add -t ssh_home_t "/var/opt/gitlab/.ssh(/.*)?"
restorecon -R -v /var/opt/gitlab/.ssh
```

4. Lihat _SELinux Context_ pada direktori `.ssh` gitlab  
Pastikan sudah berubah seperti output berikut
```bash
ls -Z /var/opt/gitlab/.ssh/authorized_keys

# Output:
# unconfined_u:object_r:ssh_home_t:s0 /var/opt/gitlab/.ssh/authorized_keys
```

5. Coba `git clone` lagi
![](/images/gitlab-git-ssh-error-3.png)

# Referensi
- [Change SELinux Context](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/sect-security-enhanced_linux-working_with_selinux-selinux_contexts_labeling_files)