---
layout: post
title: Stable Diffusion
category: Computer Vision
tag: [diffusion model, ddpm]
---

### Latent Diffusion Model (LDM)

**Diffusion Model**의 앞/뒤에 `Autoencoder`를 추가하여 sampling에 소요되는 computational cost를 대폭 줄임


#### Autoencoder의 적용 이유

- Perceptual & Semantic Compression

    - **Generative Models**은 일반적으로 `Perceptual & Semantic Compression`의 두 단계로 학습
        - 먼저 `Semantic Compression`을 수행하고, `Perceptual Compression`을 진행
        - `Semantic Compression`: 데이터의 semantic/conceptual feature가 학습되는 단계
        - `Perceptual Compression`: high frequency datail은 사라지고 약간(?)의 semantic feature를 학습한 단계

- 일반적으로 **Diffusion Model**은 `Perceptual Compression`단계에서 시간 소요가 큼
    - 그렇기 때문에 `Autoencoder`가 `Perceptual Compression`을 진행하고, **Diffusion Model**이 `Semantic Compression`을 진행하게 하자는 아이디어
    - 이는 `Perceptual Compression`을 진행하더라도 semantic feature들은 남아있기 때문에 inductive bias를 사용할 수 있다고 추측

- **LDM**이 전의 `Autoencoder`를 사용하지 않는 **Diffusio Model**들을 **Pixel Space Model**이라고 지칭


### Architecture

<img src='/assets/computer_vision/diffusion_model/papers/stable_diffusion/arch.png'>


### Training

<img src='/assets/computer_vision/diffusion_model/papers/stable_diffusion/training.png'>

### Sampling

<img src='/assets/computer_vision/diffusion_model/papers/stable_diffusion/sampling.png'>

- `Gaussian Distribution`을 통해서 `latent representation`과 동일한 사이즈의 noise와 text prompt를 encoder에 입력
- sampling결과에 대한 `denoised latent`를 decoding하여 이미지 생성
- 이 때, **CFG**의 $\gamma$로 condition 적용

### Other conditions...

### References: