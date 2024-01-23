---
layout: post
title: DeepSpeed
category: Deep Learning
tag: [deepspeed, large model training]
---

최근 NLP분야를 보면, 데이터와 모델의 크기가 커질수록 성능이 더 잘 나온다는 트렌드를 따르고 있어서 모델은 수백억 개의 파라미터로 구성되어 있을 정도로 매우 커지고 있으며, 앞으로도 새로운 기술이 나오지 않는 이상 더욱 더 커질 것으로 예상된다. 그렇기에 학습을 효율적으로 하고자 하는 연구가 필수적이다. 

### **Pytorch**에서는 `DataParallel`과 `DistributedDataParallel`을 제공한다. 

- `DataParallel`: 모델과 데이터가 각 `device`로 복사되어 학습하고, 업데이트는 다시 메인 `device`에서 진행하는 식으로 학습을 진행하기 때문에 분산학습이지만 효율적이지는 않다. 
    > `device`간의 메모리 불균형 발생
- `DistributedDataParallel`: 각 `device`를 하나의 프로세스로 보고 모델을 각 프로세스에 로드하고, 역전파를 하는 과정만 내부적으로 `gradient`를 동기화하기 때문에 메모리 불균형은 없다. 그리고 `Multi-node multi gpus`로도 학습이 가능하다. 

### [DeepSpeed](https://github.com/microsoft/DeepSpeed)

**Pytorch**에서 어느 정도 사용이 쉬운 분산학습 라이브러리를 제공해주지만, 대규모 모델에 대해서는 부족하다고 할 수 있다. 

**DeepSpeed**는 1000억 개의 파라미터를 가지는 대규모 모델을 학습할 수 있는 라이브러리 이고, **ZeRO**는 모델 및 데이터 병렬화에 필요한 리소스를 줄이는 동시에 학습할 수 있는 파라미터의 수를 늘릴 수 있는 라이브러리이다. 

기존의 분산 학습은 데이터는 각 `device`에 분산되더라도, 모델을 학습시키기 위한 `optimizer state`, `gradient`, `parameter` 등은 각 `device`에 복사해서 가지고 있어야 했다. 

- Optimizer state partitioning
- Add Gra

## References:

- [DeepSpeed](https://github.com/microsoft/DeepSpeed)