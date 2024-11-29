---
layout: post
title: Vision Transformer
category: Computer Vision
tag: [classification, transformer]
---


# [Vision Transformer](https://arxiv.org/abs/2010.11929)

#### **Transformer**는 근본적으로 `inductive bias`를 갖는다

- **CNN**은 `kernel`이라는 local filter를 활용함으로서 공간적 연속성을 고려하여 인접한 픽셀간의 상관관계를 모델링한다. 그렇기 때문에 이미지에 적합한 layer이고, 적은 데이터로도 효율적인 학습이 가능하다. 
- **ViT**는 전체 이미지를 패치로 나누어 학습하기 때문에 위치나 local context를 반영하지 않는다. 그렇기 때문에 `inductive bias`가 낮아 많은 데이터가 필요하다. 

- `ImageGPT`는 `Vision Transformer`와 가장 유사한 방법이다. 하지만 이미지 픽셀값을 토큰으로 사용하기 위해 저화질 이미지를 사용해야 한다. 따라서, `ImageNet`과 같이 중간 사이즈의 이미지 데이터셋에서는 좋은 성능이 어렵고 `Vision Transformer`에서는 `ImageGPT`와 같이 `Transformer`` 구조를 최대한 그대로 이미지 모델에 적용하되, 고화질 이미지를 학습할 수 있는 방법에 초점을 맞춘다.

### Architecture

<img src='/assets/vision_transformer/arch.png'>

#### `Patch Embedding`

<img src='/assets/vision_transformer/patch_embedding.png'>

> `ImageGPT`에서는 동일하게 만든 patch를 `BERT`와 `Autoregressive`방식으로 학습하기 위해 이미지 화질을 작게 만들었다? 픽셀값을 예측하기 위해???? 무슨 뜻인지?????????????????????

> 픽셀값을 예측할 필요가 없기 때문에 낮은 차원의 벡터로만 변환한다. 이를 위해서 `Linear Projection`을 사용한다. 


#### `Positional Embedding`

- `Transformer`에서는 Sin과 Cosine 함수를 조합하여 위치별로 **고정된** 값을 할당해준다. 이렇게 지정된 위치값은 학습이 진행되는동안 변경되지 않는다. 

- `Vision transformer`에서는 임의의 벡터값으로 초기화해준 뒤 **학습 가능한** `Positional Embedding`을 사용한다.

- `patch embedding`과 `positional embedding`의 결과는 서로 element-wise summation을 진행한다. 

    <img src='/assets/vision_transformer/patch_plus_positional.png'>

#### `Class Token Embedding`

`Vision transformer`에서는 classification을 하기 위해서 다음과 같이 `patch embedding`의 결과에서 맨 앞에 class에 관한 행을 하나 추가하고, 학습을 통해서 classification을 진행한다. 

<img src='/assets/vision_transformer/class_token_embedding.png'>

### Hybrid Architecture 

`CNN`과 `Transformer`를 함께 사용하자는 아이디어로, `linear projection`을 대신해서 `CNN`을 통해 `patch embedding`을 만든다. 

### Fine Tuning and Higher Resolution

`Vision transformer`는 매우 많은 데이터에 대해서 pretrain을 진행하고, fine-tuning을 통해서 downstream을 진행하여 사용한다. **이는 `vision transformer`가 inductive bias가 적어서 매우 많은 양의 데이터가 필요하기 때문이다.*** 

이 때, pretrain을 했던 이미지보다 더 큰 해상도로 fine-tuning을 진행하면 성능이 더 좋아지는 경우가 있다. 하지만, 해상도가 다른 이미지를 사용하게 되면, 기존의 patch의 갯수가 달라지기 때문에 `positional embedding`이 영향을 받게 된다. 그렇기 때문에 `positional embedding`에 interpolation을 사용하는 방법을 제안한다. 

### Performance Benchmark

1. 데이터셋의 양에 따른 성능 비교 

<img src='/assets/vision_transformer/pretraining_datsaet.png'>

<img src='/assets/vision_transformer/pretraining_datsaet_2.png'>

- `Vision transformer`는 데이터의 양이 커야 성능이 좋다.

2. scale에 따른 성능 비교 

<img src='/assets/vision_transformer/scale.png'>

- 동일 연산량에서는 `vision transformer`가 `resnet`보다 매우 좋다. 
- `hybrid`가 `vision transformer`보다 살짝 더 좋다. 

### Mean Attention Distance 

<img src='/assets/vision_transformer/mean_attn_dist.png'>

- `Attention distance`가 크다는 말은 더 멀리 있는 정보에 집중하고 있다는 것을 의미
- 낮은 레이어에서는 `attention distance`가 다양하게 분포하고 있는 모습으로, Local한 정보와 Global한 정보를 모두 잡아내고 있음을 의미
- 높은 레이어에서는 `attention distance`가 모두 큰 값을 갖는데, 모두 Global 정보를 잡아내고 있음을 의미

### 장점

1. 이미지의 각 부분을 독립적으로 처리하는 것이 아니라, 전체 이미지의 컨텍스트를 고려하여 정보를 처리하기 때문에 global context에 대한 인식이 좋다. 이는 Transformer의 Self Attention 모듈 덕분에 가능하며, 이로 인해 이미지의 각 부분이 서로 어떻게 관련되어 있는지, 그리고 전체 이미지에서 어떤 역할을 하는지를 파악할 수 있다. 그래서 복잡한 시각적 관계나 패턴을 파악하는 데 매우 유용하다.

2. 크기나 깊이를 쉽게 조절하여 다양한 연산 요구 사항에 맞게 스케일링할 수 있다. 

3. 이미지를 패치로 나누고 이를 벡터로 변환하는 간단한 전처리 과정만을 필요하다. 복잡한 데이터 증강이나 특별한 전처리 없이도 높은 성능을 달성할 수 있습니다. 

> augmentation도 필요없다는 건가????????????????????????????????

4. 모듈화 및 재사용의 용이성이 좋다.

### 단점

1. 이미지의 모든 부분 간의 관계를 계산하기 때문에, 계산 복잡도가 높습니다. 이로 인해, 특히 큰 이미지나 데이터셋에서는 훈련 시간이 길어질 수 있다.

2. `Inductive bias`가 낮아, 충분한 양의 데이터와 다양한 데이터가 필요하다. 

3. `Transformer` 구조는 `CNN`에 비해 직관적이지 않을 수 있다. 


## References:

- [Vision Transformer](https://arxiv.org/abs/2010.11929)

- [blog tutorial](https://ffighting.net/deep-learning-paper-review/vision-model/vision-transformer/)
