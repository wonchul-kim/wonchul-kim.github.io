---
layout: post
title: Augmentations for Object Detection
category: Computer Vision
tag: classification
---

#### Corase-grained classification vs. Fine-grained classification 

    * Coarse-grained classification: 하나의 class 내부에 대해서는 분류를 하지 않고 동일한 class로 분류
        * CIFAR10, CFIAR100, MNIST, ImageNet
    * Fine-grained classification: class 내부의 다양한 종류에 대해서도 세밀하게 분류 (예를 들어, 개를 말티즈, 불독, 등으로도 분류하는 것)
        * CUB-200-2011, Stanford Cars, Stanford Dogs