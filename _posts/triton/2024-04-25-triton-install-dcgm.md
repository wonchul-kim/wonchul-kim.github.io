---
layout: post
title: Install DCGM
category: Triton
tag: ['triton', 'dcgm']
---

## [Install DCGM](https://developer.nvidia.com/dcgm#Downloads)

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"
sudo apt-get update
sudo apt-get install -y datacenter-gpu-manager
```
