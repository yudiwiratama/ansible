---
title: "Cara Melakukan Perulangan Pada Bash"
description: "Cara mudah melakukan perulangan pada _bash script_"
date: "2021-08-04"
tags: ["Bash", "Linux"]
categories: ["Bash"]
ShowToc: true
TocOpen: true
---

# Perulangan Pada Bash
Perulangan atau _Looping_ adalah cara mudah untuk menyelesaikan pekerjaan yang sifatnya berulang-ulang, semisal ingin membuat 10 user maka kita tidak perlu menjalankan perintah `useradd` 10 kali melainkan kita membuat _Bash Script_ sebagai berikut
* Buat file dengan nama `buat-user.sh`
```bash
#!/bin/bash
for i in {1..10}; do
  sudo useradd user$i
done
```

* Hasil Bash Script
```bash
# Jalankan Script yang telah dibuat
student@latihan-bash:~$ bash buat-user.sh

# Verifikasi
student@latihan-bash:~$ getent passwd | grep user
user1:x:1002:1002::/home/user1:/bin/sh
user2:x:1003:1003::/home/user2:/bin/sh
user3:x:1004:1004::/home/user3:/bin/sh
user4:x:1005:1005::/home/user4:/bin/sh
user5:x:1006:1006::/home/user5:/bin/sh
user6:x:1007:1007::/home/user6:/bin/sh
user7:x:1008:1008::/home/user7:/bin/sh
user8:x:1009:1009::/home/user8:/bin/sh
user9:x:1010:1010::/home/user9:/bin/sh
user10:x:1011:1011::/home/user10:/bin/sh
```

# Panduan
## Perulangan tak terbatas
```bash
#!/bin/bash
while true; do
  <perintah>
done
```

* Contoh 1
```bash
#!/bin/bash
# set variabel angka sebagai 0
export angka=0

# Perulangan tak terhingga
while true; do
  echo $angka
  sleep 1
  ((angka++))
done
```

* Hasil Contoh 1
```bash
student@latihan-bash:~$ bash contoh1.sh
0
1
2
3
4
5
<akan bertambah terus sampai skrip distop>
```

## Perulangan dengan kisaran angka
```bash
#!/bin/bash
for i in {1..3}; do
  <perintah>
done
```

* Contoh 2
```bash
#!/bin/bash
for i in {1..3}; do
  echo $i
done
```

* Hasil Contoh 2
```bash
student@latihan-bash:~$ bash contoh2.sh
1
2
3
```

## Perulangan dengan daftar kata
```bash
#!/bin/bash
for <nama variabel> in <nilai 1> <nilai 2> <nilai x>; do
  echo <nama variabel>
done
```

* Contoh 3
```bash
#!/bin/bash
for kota in Jakarta Bandung Surabaya; do
  echo $kota
done
```

* Hasil Contoh 3
```bash
student@latihan-bash:~$ bash contoh3.sh
Jakarta
Bandung
Surabaya
```