---
layout: post
title: Automatic Data Curation for Self-Supervised Learning
category: Deep Learning
tag: [reduce dataset]
---



1. Self-Supervised Vision Foundation Models

이 카테고리의 모델들은 라벨 없이 이미지 표현(embedding)을 잘 학습합니다.



| 모델                           | 특징                                       | 장점                                         |
| ---------------------------- | ---------------------------------------- | ------------------------------------------ |
| **DINO / DINOv2**            | Vision Transformer 기반, self-distillation | 강력한 zero-shot 성능, semantic-aware embedding |
| **MAE (Masked Autoencoder)** | Transformer 기반, masking 기반 복원 학습         | global context에 강함                         |
| **SimCLR**                   | contrastive learning, augmentation 기반    | 단순 구조, 효과적 학습                              |
| **BYOL / MoCo v3**           | contrastive 없이도 좋은 embedding 학습          | 레이블 없이 높은 성능                               |
| **SwAV**                     | clustering + contrastive hybrid          | 클래스 분리 잘됨                                  |




2. Multimodal Foundation Models (텍스트와 함께 학습된 모델)

이런 모델은 텍스트-이미지 aligned embedding을 제공하므로, 텍스트 기반 검색/유사도 측정에 좋습니다.



| 모델                       | 특징               | 장점                                 |
| ------------------------ | ---------------- | ---------------------------------- |
| **CLIP (OpenAI)**        | 이미지와 텍스트 쌍 학습    | 다양한 zero-shot task에 강력             |
| **SigLIP (Google)**      | CLIP의 성능 개선 버전   | 더 나은 alignment                     |
| **BLIP / BLIP-2**        | 이미지 ↔ 텍스트 양방향 모델 | 이미지 captioning/embedding에 모두 사용 가능 |
| **Florence (Microsoft)** | 대규모 비전-텍스트 사전학습  | MS에서 제공하는 Vision API 기반            |


3. Object-Centric Models (Region-level embedding에 유리한 모델)

Patch-level이나 object-level 분석이 필요할 때 유리합니다.


| 모델                         | 특징                                         | 장점               |
| -------------------------- | ------------------------------------------ | ---------------- |
| **SAM (Segment Anything)** | 마스크 기반 embedding                           | 개체 단위 표현에 강함     |
| **SEEM / OneFormer**       | multi-task 표현 가능 (segmentation, caption 등) | 유연한 embedding 추출 |
| **OWL-ViT**                | object-level vision-language 모델            | 텍스트 기반 객체 탐색     |


4. Vision Transformers (일반 ViT 기반)

사전 학습된 비전 트랜스포머는 일반적으로 좋은 feature extractor로 활용 가능합니다.

| 모델                               | 특징                    | 장점                            |
| -------------------------------- | --------------------- | ----------------------------- |
| **ViT (pretrained on ImageNet)** | 기본 Transformer 구조     | Linear probing용으로 자주 활용       |
| **ConvNeXt / Swin Transformer**  | CNN+Transformer 하이브리드 | 강력한 backbone + good embedding |




--------------------------------------


* [Segment Any Anomaly without Training via Hybrid Prompt Regularization](https://arxiv.org/pdf/2305.10724)

* [WinCLIP: Zero-/Few-Shot Anomaly Classification and Segmentation](https://arxiv.org/pdf/2303.14814)

* [ANOMALYCLIP: OBJECT-AGNOSTIC PROMPT LEARNING FOR ZERO-SHOT ANOMALY DETECTION](https://openreview.net/pdf?id=buC4E91xZE)

* [Anomaly Detection Requires Better Representations](https://arxiv.org/pdf/2210.10773)
