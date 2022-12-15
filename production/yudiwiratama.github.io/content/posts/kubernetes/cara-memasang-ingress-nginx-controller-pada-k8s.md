---
title: "Cara Memasang Nginx Ingress Controller Pada Kubernetes Cluster [External IP]"
description: "Cara mudah memasang nginx ingress controller pada kubernetes cluster"
date: "2021-08-29"
tags: ["Kubernetes", "Nginx", "Ingress"]
categories: ["Kubernetes"]
ShowToc: true
TocOpen: true
---

# Pembaharuan Halaman
**2022 Juni 08:**
- Versi ingress-nginx controller v1.2.0
- Menambahkan langkah membuat ingress class

# Prasyarat
- Kubernetes Cluster

# Panduan
1. Terapkan manifest yang disediakan pada website nginx
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/baremetal/deploy.yaml
```

2. Sunting service `ingress-nginx-controller` pada namespace `ingress-nginx`

Isikan dengan IP Node kubernetes yang akan melayani _request_
```bash
kubectl -n ingress-nginx edit svc ingress-nginx-controller
```

Tambahkan baris berikut
```yaml
spec:
  externalIPs:
  - <IP_NODE_1>
  - <IP_NODE_2>
  - <IP_NODE_N>
```

3. Buat ingress class
```bash
cat<<EOF > ingressclass.yaml
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  labels:
    app.kubernetes.io/component: controller
  name: nginx
  annotations:
    ingressclass.kubernetes.io/is-default-class: "true"
spec:
  controller: k8s.io/ingress-nginx
EOF

kubectl apply -f ingressclass.yaml
```

4. Jalankan aplikasi contoh
```
# Buat deployment
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0

# Buat service
kubectl expose deployment web --port=8080

# Buat ingress
cat<<EOF > web-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
    - host: example.id
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 8080
EOF

kubectl apply -f web-ingress.yaml

# Uji coba
# Contoh: curl --resolve 'example.id:80:192.168.1.21' http://example.id
curl --resolve 'example.id:80:<Ingress_Controller_externalIPs>' http://example.id
```
![](/images/cara-memasang-ingress-nginx-controller-pada-k8s-1.png)

# Referensi
- [Deploy Nginx Controller on Baremetal](https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal)
- [Expose Nginx via ExternalIPs](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/#external-ips)
- [Set up Ingress on Minikube](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)