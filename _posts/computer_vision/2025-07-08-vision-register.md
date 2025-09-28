---
layout: post
title: VISION TRANSFORMERS NEED REGISTERS
category: Computer Vision
tag: [register]
---

# [VISION TRANSFORMERS NEED REGISTERS](https://arxiv.org/pdf/2309.16588)

* self-supervised 방식으로 학습함에도 불구하고, downstream task에 대한 성능이 좋고, unsupervised segmentation에도 가능함



#### Artifacts (outlier)

* 약 10배 더 높은 `norm` 값을 가지며, 전체의 약 2% 차지
* 주로 middle layer에 나타나며, 오래 학습하거나 모델이 큰 경우에 나타남
> discard local information 
    > * 인근 patch와 유사도가 높아 original information (position, pixel)이 포함되지 않음
    
> contain global information 
    > * patch에 classifier를 달아 예측하면 일반 patch보다 outlier patch의 성능이 더 높음
    > * 이는 outlier patch가 이미지를 global하게 이해하고 있음을 의미

> the model learns to recognize patches containing little useful information, and recycle the corresponding tokens to aggregate global image information while discarding spatial information

* 이를 해결하기 위해서 `register token`을 추가함
    * `outlier token`이 사라짐
    * `dense prediction task` 성능이 향상됨
    * `feature map`이 smooth해짐
    * **LOST**를 활용한 `object discovery` 성능도 향상됨





## References

- [VISION TRANSFORMERS NEED REGISTERS](https://arxiv.org/pdf/2309.16588)