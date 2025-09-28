---
layout: post
title: Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation
category: Computer Vision
tag: [rein]
---


# [Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation](https://arxiv.org/pdf/2012.07177v2)

- Prior studies on Copy-Paste relied on modeling the surrounding visual context for pasting the objects.

- we show Copy-Paste is additive with semi-supervised methods that leverage extra data through pseudo labeling (e.g. self-training).

- Prior work [12, 15] adopts methods for deciding where to paste the additional objects by modeling the surrounding visual context. In contrast, we find that a simple strategy of randomly picking objects and pasting them at random locations on the target image provides a significant boost on top of baselines across multiple settings such as extent of scale jittering, training schedule and image size.


- In combination with large scale jittering, we show that the Copy-Paste augmentation results in significant gains in
the data-efficiency on COCO (Figure 1).

> `data-efficiency`: 더 적은 데이터로도 비슷한 성능을 낼 수 있다.

- we show that the Copy-Paste augmentation results in better features for the two-stage training procedure
typically used in the LVIS benchmark [21].

> `two-stage training procedure`: (1) LVIS 전체 데이터셋로 기본적인 특징에 대해서 학습하고, 이 때 cross-entropy와 같은 일반적인 손실함수를 사용. (2) 1단계에서 학습한 모델을 기반으로 클래스 불균형 문제를 해결하는 기법을 적용하여 추가 학습. LVIS 데이터셋에서는 long-tail distribution 문제를 해결하기 위해 re-weighting, re-sampling 등의 기법을 적용. 


## Method 

1. randomly select two images and apply random scale jittering and random horizontal flipping on each of them. 
2. select a random subset of objects from one of the images and paste them onto the other image.
3. adjust the ground-truth annotations accordingly: 
    - remove fully occluded objects 
    - update the masks and bounding boxes of partially occluded objects


#### Large Scale Jittering (LSJ)

- Large Scale Jittering (LSJ): reisze range is 0.1 to 2.0 of the original image size
- Standard Scale Jittering (SSJ): reisze range is 0.8 to 1.25 of the original image size

LSJ is better than SSJ.

#### Blending pasted objects

No need to use blending technique such as Gaussian filter like the below:

$$
 I_1 \times \alpha + I_2 \times (1 - \alpha)
$$

- $I_1$ is the pasted image
- $I_2$ is the main image.

#### Self-training

Incorporating additional unlabeled images

1. train a supervised model with Copy-Paste augmentation on labeled data
2. generate pseudo labels on unlabeled data
3. paste ground-truth instances into pseudo labeled and supervised labeled images and train a model on this new data


## Rereferences

- [Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation](https://arxiv.org/pdf/2012.07177v2)

- [code](https://github.com/tensorflow/tpu/tree/master/models/official/detection/projects/copy_paste)
