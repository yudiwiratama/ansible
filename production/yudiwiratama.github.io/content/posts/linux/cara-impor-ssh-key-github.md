---
title: "Cara Impor SSH Key Dari Akun GitHub"
description: "Cara mudah impor ssh key dari akun github"
date: "2022-02-25"
tags: ["Linux", "SSH"]
categories: ["Linux"]
ShowToc: true
TocOpen: true
---

# Panduan

1. Simpan konten file public ssh key pada *clipboard*
```bash
cat ~/.ssh/id_rsa.pub
# simpan output
```

2. Menambahkan SSH Public Key pada akun GitHub
- Login https://github.com
- Kunjungi https://github.com/settings/keys dan klik "New SSH key"
![](/images/cara-impor-ssh-key-github.png)
- Salin key yang sudah tersimpan pada *clipboard* lalu klik "Add SSH key"
![](/images/cara-impor-ssh-key-github-2.png)

3. Import SSH Key
```bash
ssh-import-id gh:<user-id>
# Example:
# ssh-import-id gh:ajiarya
```
![](/images/cara-impor-ssh-key-github-3.png)

# Referensi
- [Launchpad ssh-import-id](https://launchpad.net/ssh-import-id)
- [man ubuntu ssh-import-id](https://manpages.ubuntu.com/manpages/bionic/man1/ssh-import-id.1.html)