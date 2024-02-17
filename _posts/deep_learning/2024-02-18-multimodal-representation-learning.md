---
layout: post
title: Multimodal Representation Learning
category: Deep Learning
tag: [multimodal]
---

# Multimodal Representation Learning

- `modality`: 이미지, 비디오, 텍스트, 대화, 그래프 등과 같이 모델을 학습하기 위한 데이터의 종류
- `unimodal model`: 하나의 `modality`만을 사용하여 학습
- `multimodal model`: 두 개 이상의 `modality`를 사용하여 학습
- `multimodal representation`: 두 개 이상의 `modality`를 하나의 feature로 나타낸 representation

    <img src='/assets/deep_learning/multimodal_representation_learning/mm_representation.png'>


## Heterogeneity Gap

- 각각의 `modality`는 매우 다른 정보로 이루어져 있기 때문에 representation에서 매우 상이할 수 있음
- 예를 들어, 이미지의 representation (in image space) 과 텍스트의 representation (in text space) 은 서로 다른 space에 존재하여 하나의 space에서는 표현하기 힘들 수 있음

- 따라서, 이러한 `Heterogeneity Gap`을 줄여서 서로 다른 `modality`를 하나의 space에 representation하는 것이 중요
    <img src='/assets/deep_learning/multimodal_representation_learning/heterogeneity_gap.png'>


### Joint Representation

- concatenative
- additive
- multiplicative

#### Visual Question Answering (VQA)
#### Visual Grounding


### Coordinated Representation

### Encoder-Decoder


### References:
- https://www.youtube.com/watch?v=PcU2oiPFDTE&ab_channel=%E2%80%8D%EA%B9%80%EC%84%B1%EB%B2%94%5B%EA%B5%90%EC%88%98%2F%EC%82%B0%EC%97%85%EA%B2%BD%EC%98%81%EA%B3%B5%ED%95%99%EB%B6%80%5D