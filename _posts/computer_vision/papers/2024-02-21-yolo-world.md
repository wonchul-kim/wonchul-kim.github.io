---
layout: post
title: YOLO-World
category: Computer Vision
tag: [yolo]
---

# YOLO-World

## Abstract

- 문제점 지적: `their reliance on predefined and trained object categories limits their applicability in open scenarios`
- 해결책 제시: `vision-language modeling and pre-training on large-scale datasets`
    - `Re-parameterizable VisionLanguage Path Aggregation Network (RepVL-PAN)`
    - `resion-text contrastive loss`
    - 결과: `detecting a wide range of objects in a zero-shot manner with high efficiency`


## Introduction
- 기존 detection: 정해진 `class`로 학습을 하면, 정해진 `class`으로만 예측 가능
- `distillation-based methods are much limited due to the scarcity of training data with a limited diversity of vocabulary`
- `region-level vision-language pretraining and train open-vocabulary object detectors at scale`
    - `heavy computation burden`
    - `complicated deployment for edge devices`


- `large-scale pre-training schemes`

- `YOLO-World follows the standard [YOLO architecture](https://github.com/ultralytics/ultralytics) and leverages the pre-trained CLIP [36] text encoder to encode the input texts.`

- `During inference, the text encoder can be removed and the text embeddings can be re-parameterized into weights of RepVL-PAN for efficient deployment.`
- `open-vocabulary pre-training scheme for YOLO detectors through region-text contrastive learning on largescale datasets, which unifies detection data, grounding data, and image-text data into region-text pairs`

- `prompt-then-detect paradigm`: `encodes the prompts of a user to build an offline vocabulary and the vocabulary varies with different needs. Then, the efficient detector can infer the offline vocabulary on the fly without re-encoding the prompts. For practical applications, once we have trained the detector, i.e., YOLO-World, we can pre-encode the prompts or categories to build an offline vocabulary and then seamlessly integrate it into the detector.`

