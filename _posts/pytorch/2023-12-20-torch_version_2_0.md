---
layout: post
title: Set environments for Pytorch 2.x
category: Pytorch
tag: pytorch2.x
---

# In order to use pytorch2.x with GPUs, NVML has to do with it!

As I know, I couldn't use gpus for `torch==2.x` under `NVIDIA driver 470.xxx.xxx`.
Thus, I installed `NVIDIA driver 535.xxx.xxx`.


### Environment

- cuda11.8-cudnn8.0-devel-ubuntu20.04
- torch==2.1.2

> Even though torch is installed, You should check `torch.cuda.is_available()` is `True`!



