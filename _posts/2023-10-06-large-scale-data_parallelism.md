---
layout: post
title: Data Parallel
category: Large Scale
tag: large-scale, Data Parallel
---

## `torch.nn.DataParallel`

**single node의 multiple GPUs**에서 parallel training이 가능한 multi-thread 모듈이다.

### Forward Pass 

<img src='./imgs/large_scale/dp_forward.png>

1. mini-batch를 `Scatter`를 통해 각 device(GPU)로 전송
2. main device에 로드되어 있는 모델의 파라미터를 `Broadcast`를 통해 각 device에 복사
3. 각 device에 전송된 mini-batch를 복사된 모델의 파라미터에 대해 forward하여 logits를 계산
4. 계산된 logits를 `Gather`를 통해 main device에 모음
5. 모은 logits를 이용해서 loss를 계산


### Backward Pass 

<img src='./imgs/large_scale/dp_backward.png>

1. 계산된 loss를 각 device에 `Scatter`를 통해 전송
2. 각 device에서는 전송된 loss로 backward로 gradiens 계산
3. 계산된 각 device에서의 gradients를 `Reduce`를 통해 main device로 전송하여 더함
4. 더해진 gradients로 main device의 모델을 업데이트


### 문제점

#### 1. 메모리 사용 불균형

모델 업데이트를 위해서 각 device에서 계산된 logits를 main device에서 처리하기 때문에 main device의 메모리를 다른 device보다 많이 사용하게 된다.

이는 **[Pytorch-Encoding](https://github.com/zhanghang1989/PyTorch-Encoding)**과 같이 device에 logits이 아닌 loss를 `Gather`함으로서 완화할 수 있다. 

> logits는 vector이나 loss는 scalar이기 때문에 크기가 훨씬 작기 때문이다.

<img src='./imgs/large_scale/dp_forward_2.png>

결국에는 logits로부터 loss를 계산하는 것도 multi-thread에서 수행하는 것이다. 이 때, loss에 대한 reduction이 2번 (그림에서 4, 5) 발생하게 되는 단점이 있으나, 메모리 불균형을 해결하고 loss computation에 대한 부분을 병렬화할 수 있다는 장점이 더 크게 작용한다.


#### 2. multi-thread 모듈이기 때문에 python에더 비효율적

Python은 GIL에 의해서 하나의 process에서 여러 개의 thread가 동시에 동작할 수 없다. 그렇기 때문에 multi-thread가 아닌 multi-processing 으로 해야 한다.

#### 3. 매 step마다 device간의 데이터 전송이 이루어져야 함

