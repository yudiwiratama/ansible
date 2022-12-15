---
title: "Cara Memasang gVisor Pada Kubernetes (Containerd)"
description: "Cara memasang gVisor pada Kubernetes "
date: "2022-06-24"
tags: ["Kubernetes", "Runtime"]
categories: ["Kubernetes"]
ShowToc: true
TocOpen: true
---

# Lingkungan
- Sistem Operasi: Ubuntu 20.04 LTS Cloud Image
- Kubernetes: 1.22.1
- Containerd: 1.6.6

# Panduan
1. Pasang repository gVisor
```bash
curl -fsSL https://gvisor.dev/archive.key | gpg --dearmor -o /usr/share/keyrings/gvisor-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/gvisor-archive-keyring.gpg] https://storage.googleapis.com/gvisor/releases release main" | tee /etc/apt/sources.list.d/gvisor.list > /dev/null
```

2. Install runsc (gVisor)
```bash
apt update -y
apt install -y runsc
```

3. Definisikan runsc pada konfigurasi containerd
```bash
vim /etc/containerd/config.toml
```

Tambahkan baris berikut pada bagian [plugins]
```diff
---konten dari file dipotong---
[plugins]

---konten dari file dipotong---
# Tambahkan baris berikut
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runsc]
    runtime_type = "io.containerd.runsc.v1"
# Akhir dari baris yang perlu ditambahkan

[proxy_plugins]
---konten dari file dipotong---
```

4. Restart containerd
```bash
systemctl restart containerd
```

5. Tambahkan objek runtimeclass pada Kubernetes
```bash
cat<<EOF > gvisor-runtime.yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
EOF

kubectl apply -f gvisor-runtime.yaml
```

6. Membuat pod dengan runtime gVisor
```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx-gvisor
spec:
  runtimeClassName: gvisor
  containers:
  - name: nginx
    image: nginx
EOF
```

# Periksa Kernel
- host
```bash
uname -a
Linux aa-kubernetes-master01 5.4.0-120-generic #136-Ubuntu SMP Fri Jun 10 13:40:48 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

- runc - containerd
```bash
kubectl exec -it web -- uname -a
Linux web 5.4.0-120-generic #136-Ubuntu SMP Fri Jun 10 13:40:48 UTC 2022 x86_64 GNU/Linux
```

- runsc - gVisor
```bash
kubectl exec -it nginx-gvisor -- uname -a
Linux nginx-gvisor 4.4.0 #1 SMP Sun Jan 10 15:06:54 PST 2016 x86_64 GNU/Linux
```

Terlihat containerd menggunakan kernel yang sama dengan host sedangkan gVisor menggunakan kernel berbeda, ini dikarenakan gVisor melakukan isolasi sehingga kernel yang digunakan oleh container bersifat independen dengan maksud untuk meningkatkan keamanan terhadap eksploitasi Kernel Host atau serangan seperti *container escape*.

![](https://gvisor.dev/docs/Layers.png)
Sumber Gambar: https://gvisor.dev/docs/Layers.png

# Referensi
- [What is gVisor?](https://gvisor.dev/docs/)
- [gVisor - Install](https://gvisor.dev/docs/user_guide/install/)
- [gVisor - Containerd Quick Start](https://gvisor.dev/docs/user_guide/containerd/quick_start/)
- [Kubernetes Docs - Runtime Class](https://kubernetes.io/docs/concepts/containers/runtime-class/)