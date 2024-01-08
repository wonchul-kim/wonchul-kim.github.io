---
layout: post
title: Parallel Processing
category: Pytorch
tag: large-scale, Parallel Processing
---

## Parallel Processing

병렬처리를 통해서 속도와 메모리의 효율성을 높일 수 있다.


### Data Parallel

<img src='/assets/large_scale/data_parallel.png'>

각 GPU에 모델을 복사하고, 데이터는 batch-size만큼 분배하여 학습을 하는 방식으로 GPU 갯수만큼 batch-size를 더 크게 할 수 있다. 


### Model Parallel

<img src='/assets/large_scale/model_parallel.png'>

만약, 모델이 하나의 GPU에 올라갈 수 없을 정도로 크다면, 모델을 내부 파리미터 단위로 하여금 여러 
GPU에 분배하여 학습하는 방식이다.

#### Inter-layder Model Parallel 
 
<img src='/assets/large_scale/inter_layer.png'>

layer 단위로 모델을 분리하여 각 GPU에 올려서 학습하는 방식이다.

#### Intra-layer Model Parallel 
 
<img src='/assets/large_scale/intra_layer.png'>

layer와 관계없이 tensor 자체를 쪼개는 방식으로 **NVIDIA의 Megatron-LM**이 이데 해당한다. 예를 들어, [256, 256] 사이즈의 parameter는 [128, 256] 또는 [256, 128]로 쪼갠다. 


#### Pipeline Parallel

inter-layer 방식의 단점을 개선한 방법으로 inter-layer는 layer 단위로 쪼개지면 각 layer가 학습되어야 하는 순서가 존재한다. 예를 들어, 1번 layer가 진행되어야 2번 layer가 진행되고, ... N번 layer가 순차적으로 진행되기 때문에 모델을 쪼개서 학습하는 것 자체는 병렬화라고 하지만 완전 병렬 처리는 아니라고 할 수 있다. 

<img src='/assets/large_scale/pipeline_parallel_1.png'>

이런 문제점을 해결하기 위해서 아래 그림처럼 연산과정을 병렬적으로 파이프라인을 구성하는 방식이다.

<img src='/assets/large_scale/pipeline_parallel_2.png'>


#### Multi-dimension parallel

<img src='/assets/large_scale/multi_dimension_parallel.png'>

앞선 병렬처리 방법들은 동시에 여러 개를 적용할 수 있으며, 각각의 방법은 하나의 dimension을 의마할 수 있다. 

#### ZeRO (Zero Redundancy Optimization)