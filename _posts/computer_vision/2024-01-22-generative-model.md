---
layout: post
title: Generative Model
category: Computer Vision
tag: [generative]
---

### 1. Generative Advarsarial Network (GAN)

<img src='/assets/computer_vision/generative_model/gan.png'>

- 장점: high fidelity
    - 실제 사진과 유사한 이미지를 생성하는 데에 있어서 성능이 좋다. 
    - 모델 구조의 선택이 자유롭다.

- 단점: less diversity
    - 사실적인 이미지를 생성하지만, 학습한 사진 이외의 다양한 이미지를 생성하지 못한다. 
    - 학습 과정이 불안하다.


### 2. Variational Autoencoder (VAE)

<img src='/assets/computer_vision/generative_model/vae.png'>

- 장점:

- 단점: 


### 3. Likelihood Based Model

#### 3-1. Flow-based Model

<img src='/assets/computer_vision/generative_model/flow.png'>

**change of variable theorem**에 기반한 방법으로, `log-likelihood objective`로 학습한다. 

#### 3-2. Autoregressive Model 

**DALL.E**처럼 이미지 패치와 `transformer decoder`를 활용한다.

### 4. Diffusion Model

<img src='/assets/computer_vision/generative_model/diffusion.png'>

- 장점: 
    - **GAN**과는 다르게 `stationary objective`로 학습한다.
    - 모델의 scaling이 자유롭다.
    - distirubtion coverage가 커서 다양한 이미지를 생성할 수 있다. 

- 단점: 
    - 순차적인 inverse process를 통해 이미지를 생성하기 때문에 생성속도가 느리다.
    - GAN에 비해 fidelity가 낮다. 

