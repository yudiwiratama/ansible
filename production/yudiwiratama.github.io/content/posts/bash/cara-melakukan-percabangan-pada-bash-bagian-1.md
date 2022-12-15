---
title: "Cara Melakukan Percabangan Dasar Bash - Bagian 1 [Operator Aritmatika]"
description: "Cara mudah melakukan percabangan pada bash script bagian 1"
date: "2021-08-12"
tags: ["Bash", "Linux"]
categories: ["Bash"]
ShowToc: true
TocOpen: true
---

# Pendahuluan
Jika kita ingin melakukan pengecekan terhadap suatu variabel maka kita membutuhkan yang namanya percabangan sebagai contoh, jika (`if`) variabel `umur` kurang dari `5` maka sama dengan `balita`, untuk melakukan hal tersebut berikut contoh singkatnya

* `balita.sh`
```bash
#!/bin/bash

export umur=4

# Cara 1
echo "Menjalankan cara 1"
if [[ umur -lt 5 ]]; then
  echo "Kamu balita!"
fi

# Cara 2
echo "Menjalankan cara 2"
if (( umur >= 5 )); then
  echo "Kamu balita!"
fi
```

Jika kita menjankankan contoh diatas maka keluarannya adalah seperti berikut
```bash
[student@debug-bash bash]$ bash balita.sh
Kamu balita!
```

# Panduan
* Penggunaan `IF`, `ELIF`, & `ELSE`
```bash
#!/bin/bash
echo -n "Masukkan umur kamu "
read umur

# Cara 1
echo "Menjalankan cara 1"
if [[ umur -eq 0 ]]; then
  echo "kamu baru lahir!"
elif [[ umur -ge 20 ]]; then
  echo "kamu sudah dewasa!"
else
  echo "aku tidak mengerti"
fi

# Cara 2
echo "Menjalankan cara 2"
if (( umur == 0 )); then
  echo "kamu baru lahir!"
elif (( umur >= 20 )); then
  echo "kamu sudah dewasa!"
else
  echo "aku tidak mengerti"
fi
```

# Catatan

## Khusus numerik
### Syntax
- `[[ a <operator> b ]]`

### Contoh
- `[[ a -eq b ]]`

### Operator
- `-eq` = sama dengan 
- `-ne` = tidak sama dengan
- `-lt` = kurang dari
- `-le` = kurang dari atau sama dengan
- `-gt` = lebih dari
- `-ge` = lebih dari atau sama dengan

> **Tips**  
> Agar mudah mengingat berikut caranya
> - eq = equal
> - ne = not equal
> - lt = less than
> - le = less equal
> - gt = greater than
> - ge = greater equal

## Bisa numerik dan string
### Syntax
- `(( a <operator> b ))`

### Contoh
- `(( a == b ))`

### Operator
- `==` = sama dengan 
- `!=` = tidak sama dengan
- `<`  = kurang dari
- `<=` = kurang dari atau sama dengan
- `>`  = lebih dari
- `>=` = lebih dari atau sama dengan

# Referensi
- [GNU Bash Manual - Bash Conditional Expressions](https://www.gnu.org/software/bash/manual/bash.html#Bash-Conditional-Expressions)
- [GNU Bash Manual - Shell Aritmetic](https://www.gnu.org/software/bash/manual/bash.html#Shell-Arithmetic)