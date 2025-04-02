---
layout: post
title: Mask2Former
category: Computer Vision
tag: [mask2former]
---


# [Masked-attention Mask Transformer for Universal Image Segmentation](https://arxiv.org/abs/2112.01527) [code] (https://github.com/facebookresearch/Mask2Former)


- MaskFormer의 한계점을 개선한 논문
    
    - Instance / Semantic / Panoptic segmentation을 하나의 모델로 통합한 모델
    
        - 오히려 통합했다는 점에서 성능면에서는 한계가 존재
    
    - DETR을 토대로 설계되어 수렴에 긴 시간이 걸는 한계가 있음


이를 위해서 크게 다음의 3가지를 제안

1. Masked Attetion

    기존의 attention은 모든 픽셀에 대해서 연산하였지만, 이는 의미있는 영역에만 부분적으로 연산하여 수렴을 가속함

2. Multi-scale

    작은 객체에 대해서도 성능을 높임

3. 최적화

    - Self-attention, Cross-attention의 위치변화

    - query feature에 대한 학습

    - dropout 제거


### Architecture 

#### Bckbone
    
    `ResNet`

#### Pixel Decoder

    - `Multi-scale Deformable Attention Transformer (MSDeformAttn)`을 사용함

    - `ResNet`의 multi-scale(1/8, 1/16, 1/32)의 feature map들을 1-D로 결합하여 전체 해상도에 대한 Deformable self-attention을 하고, 이를 다시 1/8, 1/16, 1/32의 scale로 복원

    - `position embedding`과 `scale-level embedding`을 포함

    - 1/4 scale의 해상도는 단순히 1/8 scale feature로부터 선형보간하여 생성

#### Transformer Decoder

    - query에 대한 지역전인 부분을 학습하는 것이 목적

    - **Masked Attention**: 