---
layout: post
title: Large Scale Intro. 
category: Pytorch
tag: large-scale
---

### 모델의 architecture보다 모델의 크기와 데이터의 크기가 성능에 더 큰 영향을 미친다.

1. 모델의 크기
2. 데이터의 크기

하지만, 단순히 모델의 크기가 키운다고 해서 해결되는 것이 아니라, 모델의 크기가 커진만큼 학습을 하기 위한 techqnique(?)이 요구된다. 

- TensorRT: GPU 기반 배포기술
- DeepSpeed: 메모리 최적화
- Fused Kernel: 고성능 GPU 연산
- Megatron LM: 모델 병렬처리
- Big Query: 빅데이터 처리

