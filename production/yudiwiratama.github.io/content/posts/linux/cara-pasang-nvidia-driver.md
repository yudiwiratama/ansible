---
title: "Cara Pasang NVIDIA Driver dan Library CUDNN"
description: "Cara pasang nvidia driver"
date: "2021-11-01"
tags: ["Linux", "NVIDIA", "GPU"]
categories: ["NVIDIA"]
ShowToc: true
TocOpen: true
---

# Prasyarat
- Akun NVIDIA Developer

# Panduan
1. Unduh cuda [disini](https://developer.nvidia.com/cuda-toolkit-archive) dan cudnn [disini](https://developer.nvidia.com/cudnn)
```bash
# Create directory
mkdir /root/nvidia; cd /root/nvidia/

# cuda
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
wget https://developer.download.nvidia.com/compute/cuda/11.4.2/local_installers/cuda-repo-ubuntu1804-11-4-local_11.4.2-470.57.02-1_amd64.deb

# cudnn
wget -O cudnn-11.4-linux-x64-v8.2.4.15.tgz \
    https://developer.nvidia.com/compute/machine-learning/cudnn/secure/8.2.4/11.4_20210831/cudnn-11.4-linux-x64-v8.2.4.15.tgz?<download_token>
wget -O libcudnn8_8.2.4.15-1+cuda11.4_amd64.deb \
    https://developer.download.nvidia.com/compute/machine-learning/cudnn/secure/8.2.4/11.4_20210831/cudnn-11.4-linux-x64-v8.2.4.15.tgz?<download_token>
wget -O libcudnn8-dev_8.2.4.15-1+cuda11.4_amd64.deb \
    https://developer.download.nvidia.com/compute/machine-learning/cudnn/secure/8.2.4/11.4_20210831/Ubuntu18_04-x64/libcudnn8-dev_8.2.4.15-1%2Bcuda11.4_amd64.deb?<download_token>
wget -O libcudnn8-samples_8.2.4.15-1+cuda11.4_amd64.deb \
    https://developer.download.nvidia.com/compute/machine-learning/cudnn/secure/8.2.4/11.4_20210831/Ubuntu18_04-x64/libcudnn8-samples_8.2.4.15-1%2Bcuda11.4_amd64.deb?<download_token>
```

2. Pasang cuda
```bash
mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
dpkg -i cuda-repo-ubuntu1804-11-4-local_11.4.2-470.57.02-1_amd64.deb
apt-key add /var/cuda-repo-ubuntu1804-11-4-local/7fa2af80.pub
apt-get update
apt-get -y install cuda
```

3. Pasang cudnn library
```bash
# pasang cudnn library file
tar -xzvf cudnn-11.4-linux-x64-v8.2.4.15.tgz
cp cuda/include/cudnn*.h /usr/local/cuda/include 
cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64 
chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

# pasang cudnn deb package
dpkg -i libcudnn8_8.2.4.15-1+cuda11.4_amd64.deb
dpkg -i libcudnn8-dev_8.2.4.15-1+cuda11.4_amd64.deb
dpkg -i libcudnn8-samples_8.2.4.15-1+cuda11.4_amd64.deb
```

4. Verifikasi driver NVIDIA 
```bash
nvidia-smi
```
![](/images/nvidia-driver.png)

# Referensi
- [CUDA Documentation](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html#linux)
- [CUDNN Documentation](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html)