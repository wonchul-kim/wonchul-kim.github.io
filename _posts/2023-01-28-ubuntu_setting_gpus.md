---
layout: post
title: Setting GPUs
category: ubuntu
tag: ubuntu gpus setting
---

## To install nvidia-driver for GPUs

0. check the available nvidia-drivers

```
ubuntu-drivers devices
```

- 현재 사용중인 그래픽카드 확인
  ```
  lshw -numeric -C display
  lspci | grep -i nvidia
  ```

1. automatically install

```
sudo ubuntu-drivers autointall
```

or install specific version of driver

```
sudo apt install nvidia-driver-xxx
```

2. install cuda

3. install cudnn

4. install libraries

- pytorch for rtx3XXX
  ```
  pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
  ```
- tensorflow
  ```
  pip install tensorflow-gpu==2.4 -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
  ```
  ```
  tf.test.is_gpu_available()
  tf.config.list_physical_devices()
  ```

