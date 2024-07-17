---
layout: post
title: torch.backends.cudnn.benchmark
category: Pytorch
tag: cudnn
---


```python
torch.backends.cudnn.benchmark = True
```

#### CUDNN Convolution 연산을 최적화하기 위함

- 성능 향상: 최적화를 통해서 성능이 향상될 수 있으며, 특히 입력 데이터의 크기나 형태가 변하지 않는 경우에 유용

- 자동 튜닝: `benchmark` 모드에서는 CUDA 연산의 실행시간을 측정하여 최적의 연산 방법을 자동으로 선택함

- 메모리 사용 최적화: `cudnn`은 메모리 사용을 최적화하여 GPU메모리를 효율적으로 관리하므로, 대규모 모델 테스트에 유용

- 초기에 최적화에 따른 시간 소요가 있을 수 있음