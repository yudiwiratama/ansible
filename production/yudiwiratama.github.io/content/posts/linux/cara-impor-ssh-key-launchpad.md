---
title: "Cara Impor SSH Key Dari Akun Launchpad"
description: "Cara mudah impor ssh key dari akun launchpad"
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

2. Menambahkan SSH Public Key pada akun Launchpad
- Kunjungi https://launchpad.net/~\<user-id\>
- Klik "SSH keys: +"
![](/images/cara-impor-ssh-key-launchpad.png)
- Salin key yang sudah tersimpan pada *clipboard* lalu klik "Import Public Key"
![](/images/cara-impor-ssh-key-launchpad-2.png)

3. Import SSH Key
```bash
ssh-import-id lp:<user-id>
# Example:
# ssh-import-id lp:aryasaurus
```
![](/images/cara-impor-ssh-key-launchpad-3.png)

# Referensi
- [Launchpad ssh-import-id](https://launchpad.net/ssh-import-id)
- [man ubuntu ssh-import-id](https://manpages.ubuntu.com/manpages/bionic/man1/ssh-import-id.1.html)