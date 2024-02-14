---
layout: post
title: Diffusion Models Beat GANs on Image Synthesis (Classifier Guided Diffusion)
category: Computer Vision
tag: [generative, diffusion]
---

# [Diffusion Models Beat GANs on Image Synthesis](https://arxiv.org/pdf/2105.05233.pdf)

- `class` 정보를 condition으로 입력받아 해당 `class` 이미지를 생성할 수 있도록 하는 **Diffusion Model**을 제안 

- 별도의 `classifier`를 학습시켜서 이로부터 나오는 gradient를 활용

## Abstract

- improve sample quality with classifier guidance: a simple, compute-efficient method for trading off diversity for fidelity using
gradients from a classifier. 

- classifier guidance combines well with upsampling diffusion models, further improving FID.
    > 왜 upsampling에서만 ??? feature를 만드는 과정이 아닌 feature를 복원하는 과정에서만 적용?

## Introduction

-  sample quality metrics such as FID, Inception Score and Precision [Improved precision and recall metric for assessing generative models].
    -  However, some of these metrics do not fully capture diversity, and it has been shown that GANs capture less diversity
than state-of-the-art likelihood-based models.
    - Furthermore, GANs are often difficult to train, collapsing without carefully selected hyperparameters and regularizers [5, 41, 4]

- Diffusion models are a class of likelihood-based models offering distribution coverage, a stationary training objective, and easy scalability

- `We hypothesize that the gap between diffusion models and GANs stems from at least two factors: first, that the model architectures used by recent GAN literature have been heavily explored and refined; second, that GANs are able to trade off diversity for fidelity, producing high quality samples but not covering the whole distribution. We aim to bring these benefits to diffusion models, first by improving model architecture and then by devising a scheme for trading off diversity for fidelity` 


## References
- [Diffusion Models Beat GANs on Image Synthesis](https://arxiv.org/pdf/2105.05233.pdf)