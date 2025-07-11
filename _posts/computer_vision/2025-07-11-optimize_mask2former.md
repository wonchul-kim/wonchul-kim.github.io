---
layout: post
title: Optimizer Mask2Former for TensorRT
category: Computer Vision
tag: [mask2former trt]
---


#### 원인 파악 전 할 테스트

1. `TensorRT`로 cpp 환경에서 테스트
    - one-batch vs. multi-batch: 속도가 느려지는지 파악

2. multi-batch에서 속도가 느려진다면, python 환경에서 테스트
    - one-batch vs. multi-batch: 속도가 느려지는지 파악

3. 속도가 느려진다면, 모델 자체의 원인으로 규명하고, 모델 최적화 진행



#### 모델 최적화 for TensorRT

1. **Mask2Former**는 세그멘테이션에 대한 특수 모델로 `decoder`가 생명이므로 `encoder`를 **resnet**계열로 tensorRT 테스트

2-1. 그럼에도 속도가 느리다면, 이는 `decoder`문제로 규명

	- `decoder`의 transformer 개선하기

2-2. 빨리진다면, 'ecoder' 문제로 규명

	- **swin transformer** 개선하기