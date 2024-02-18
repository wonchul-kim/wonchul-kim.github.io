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



#### Classifier Guidance 
- `score function`: $\epsilon$이 noise를 예측하는 값이라고 할 때, 이에 대한 scaling을 적용한 값으로 계산
    $$
    \nabla logp(\mathbf{x_t}) = -\frac{1}{\sqrt{1 - \bar{\alpha_t}}}\mathbf{\epsilon_\theta}(\mathbf{x_t})
    $$

    - 이는 특정 시점의 이미지($\mathbf{x_t}$)가 주어졌을 때, `score function`에 따른 gradient로 학습을 하면, `likelihood`가 높아진다는 것을 의미하는 함수

- condition($y$)을 추가하여 다음과 같이 `Bayes Rule`로 전개하면 다음과 같다.

    $$
    logp(\mathbf{x_t}|y) = log(\frac{p(\mathbf{x_t})p(y|\mathbf{x_t})}{p(y)}) \\
    = logp(\mathbf{x_t}) + log p(y|\mathbf{x_t}) - logp(y) \\
    $$

    이에 대해서 $y$에 대해 미분을 하면, 
    $$
    \nabla_y logp(\mathbf{x_t}|y) = \nabla_y logp(\mathbf{x_t}) + \nabla_y logp(y|\mathbf{x_t})
    $$

    - $\nabla_y logp(\mathbf{x_t})$: unconditional score
    - $\nabla_y logp(y|\mathbf{x_t})$: adversarial gradient

    이를 통해서, sampling할 때 gradient만으로도 가능하다. 즉, 모델을 학습할 때, 직접적으로 condition을 고려할 필요없이 학습된 모델의 classifier의 gradient만으로도 condition이 적용이 가능하며, 이를 `classifier guidance`라고 한다.

 - 단점: 
    1. 학습한 diffusion model만으로 sampling이 불가능하고, 추가적인 classifier model이 필요
    2. 모든 noise level에 대해서 학습된 classifier가 필요하기 때문에 pretrained model의 사용이 불가

    > 기존 diffusion model만으로 conditional sampling을 하기 위한 해결책으로 **Classifier Free Guaidance**라는 논문이 나옴



## References
- [Diffusion Models Beat GANs on Image Synthesis](https://arxiv.org/pdf/2105.05233.pdf)