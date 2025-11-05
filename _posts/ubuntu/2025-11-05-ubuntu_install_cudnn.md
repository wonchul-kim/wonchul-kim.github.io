---
layout: post
title: Install cuDNN
category: Ubuntu
tag: cudnn
---


#### 1. Download

`https://developer.nvidia.com/cudnn-archive`에서 원하는 버전의 **cuDNN**을 `tar`로 다운받고 압축을 푼다.

#### 2. Install
```sh
sudo cp include/cudnn*.h /usr/local/cuda/include/
sudo cp -P lib/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn*.h
```

#### 3. Check
```sh
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```