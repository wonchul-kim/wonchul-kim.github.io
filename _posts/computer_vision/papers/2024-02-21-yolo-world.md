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


## 1.Introduction
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

## 3.Method 

### 3.1. Region-Text Pairs

$$
intput: \ \Omega = \{B_i, t_i\}^M_{i=1}
$$


$$
output: \{\hat{B_k}\}, \ \{e_k\} (e_k \in \mathbb{R})
$$

* $\{e_k\}$: corresponding object embeddings  
> ????????????????????????????????????????


### 3.2. Model Architecture

#### YOLO detector
- An image encoder 
- **path aggregation network (PAN)** for multi-scale feature pyramids
- **head** for bounding box regression and object embeddings

#### Text Encoder
$$
    W = TextEncoder(T) \in \mathbb{R}^{C*D}
$$

- transformer text encoder pre-trained by **CLIP**

#### Re-parameterizable Vision-Language Path Aggregation Network (RepVL-PAN)

#### Text Contrastive HEAD
- to obtain the object-text similarity $s_{k,j}$

- the decoupled head with two 3×3 convs to regress bounding boxes $\{b_k\}^K_{k=1}$ and object embeddings $\{e_k\}^K_{k=1}$, where $K$ denotes the number of objects

$$
s_{k,j} = \alpha*L2-Norm(e_k)*L2-Norm(w_j)^T + \beta
$$

- $w_j \in W$: ***j***-th text embeddings
- affine transformation with the learnable scaling factor $\alpha$ and shifting factor $\beta$


#### Training with Online Vocabulary
- `sample all positive nouns involved in the mosaic images and randomly sample some negative nouns from the corresponding dataset`
    > ????????????????????????????????????????????????????? 
- `The vocabulary for each mosaic sample contains at most M nouns, and M is set to 80 as default`
    
#### Inference with Offline Vocabulary
- `prompt-then-detect` strategy
- avoiding computation for each input
- flexibility to adjust the vocabulary as needed

### 3.3. Re-parameterizable Vision-Language PAN

#### Text-guided CSPLayer
$$
    X'_l = X_l*\delta(max_{j \in \{1...C\}} (X_lW^T_l))^T
$$

- $\delta$: sigmoid function
- incorporate text guidance into multi-scale image features

#### Image-Pooling Attention