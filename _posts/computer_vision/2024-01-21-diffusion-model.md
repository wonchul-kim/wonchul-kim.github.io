---
layout: post
title: Diffusion Model Basics
category: Computer Vision
tag: [diffusion]
---

### Image Generative Model

- GAN: Generator & Discriminator

- Diffusion

> 단순히 이미지를 생성하는 것이 아닌, 텍스트로부터 기존에 학습하지 않았던 새로운 이미지를 생성할 수 있다. (e.g. DALLE2, GLIGEN)

### Diffusion Process 

여러 단게를 거쳐서 입력 이미지($x_0$)에 노이즈($\epsilon$)를 추가한다고 할 때, 이 노이즈가 `Gaussian` 분포를 따른다고 가정한다. 

$$
x_0 \rarr x_1 = x_0 + \epsilon \rarr x_2 = x_1 + \epsilon \rarr ... \rarr x_{T} = x_{T-1} + \epsilon \\
\epsilon \sim N(\mu, \sigma)
$$

즉, $t$ 시간마다 추가되는 노이즈를 계산할 수 있다면 위의 과정을 역(`Reverse Diffusion Process`)으로 진행하여 `Noise` 이미지로부터 이미지를 생성할 수 있다. 

### Diffusion Model Architecture 

<img src='/assets/computer_vision/diffucion_model/diffusion_model_arch.png'>

- `input`은 이미지 또는 노이즈와 같은 2/3차원의 array
- 노이즈는 $t$시간의 특정 노이즈를 예측하는 것이기 때문에 $t$가 주어져야 한다.
- `condition`이 있다면, **Diffusion Model`에 주어저야 한다. 
- `output`은 `input`과 동일한 차원으로 **Diffusion Model** 자체는 `UNet` 구조를 따른다. 

### Loss Function

$$
L_{simple}(\theta)  = \mathbb{E}_{t, x_0, \epsilon}[|| \epsilon - \epsilon_{theta}(\sqrt{\bar{\alpha_t}}\mathbf{X_0} + \sqrt{1 - \bar{\alpha_t}}\epsilon, t)||^2]
$$

- $\epsilon$: 실제 노이즈
- $\epsilon_{theta}(\sqrt{\bar{\alpha_t}}\mathbf{X_0} + \sqrt{1 - \bar{\alpha_t}}\epsilon, t)$: 모델이 예측한 노이즈


### Referemces:
- https://ffighting.net/deep-learning-paper-review/diffusion-model/diffusion-model-basic/