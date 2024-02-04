---
layout: post
title: How Diffusion Models Work
category: Computer Vision
tag: [diffusion model]
---

# How Diffusion Models Work

- **DDPM**
- **DDIM**

### Sampling

1. noise 이미지를 input으로 하여 모델은 noise를 예측
    > 이 때의 `noise image`는 normal distribution을 따른다.
2. 예측한 노이즈를 input 이미지로부터 추출(`denosing`)하고 `extra noise`를 더해준다. 
    > `denoised image`는 더 이상 normal distribution을 따르지 않는다. 
    > `extra noise`는 `scaling factor`를 적용하여 더해진다. 
3. 2번에서의 결과 image를 다시 모델에 입력하여 반복

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/sampling.png" 
        alt="Picture" 
        width="600" 
        height="400" 
        style="display: block; margin: 0 auto" />

위의 과정에서는 나타나 있지 않지만, 실제로는 `extra noise`를 `denoised image`에 아래와 같이 더해준다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/sampling2.png" 
        alt="Picture" 


위의 과정을 코드로 보면, 모델은 주어진 noise image에서 추출한 noise를 예측하고, 이 예측한 noise를 input 이미지(noise image)로부터 추출한다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/sampling_code.png" 
        alt="Picture" 
        width="600" 
        height="500" 
        style="display: block; margin: 0 auto" />


#### `extra noise`는 empirically 모델을 안정화하여 유/무에 따라서 성능 차이가 난다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/extra_noise.png" alt="Picture"/>

> 아무래도 overfitting을 방지할 수도 있고 좀 더 robust results를 도출할 수 있는 방법인거 같다. 

--------------------------------------------------------
### Neural Network

#### UNet

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/unet.png" alt="Picture"
        width="600" 
        height="400" 
        style="display: block; margin: 0 auto"/>

- `input`/`output`의 dimension이 동일하다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/embedding.png" alt="Picture"
        width="800" 
        height="300" 
        style="display: block; margin: 0 auto"/>

- `ecoding`과 `decoding`의 `embedding`에 원하는 추가 정보를 병합할 수 있다. 
    - `time embedding`: `timestep`으로 noise level을 결정하는데에 도움이 된다. 
        - summation으로 적용
    - 'context embeding`: e.g. class(label) 정보 
        - multiplication으로 적용 

--------------------------------------------------------
### Training 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/training1.png" alt="Picture"
        width="800" 
        height="300" 
        style="display: block; margin: 0 auto"/>

1. 원본 이미지에 randomly generated noise by the timestep (noise level)을 적용
    > 이 때, 일정한 noise level (timestep)이 아니라 randomly selected timestep을 적용
2. 1의 noise image로부터 모델이 noise를 예측
3. 예측한 noise와 실제 1에서 추가한 noise간의 차이를 통해서 loss 계산하여 모델 업데이트

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/training1.png" alt="Picture"
        width="800" 
        height="300" 
        style="display: block; margin: 0 auto"/>

이를 코드로 작성하면 아래와 같다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/training2.png" alt="Picture"
        width="700" 
        height="500" 
        style="display: block; margin: 0 auto"/>

--------------------------------------------------------
### Controlling

#### Embeddings

- `Context Embedding`

아래의 그림과 같이 `context embedding`을 추가해주면서 학습을 할 수 있다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/context_embedding.png" alt="Picture"
        width="700" 
        height="400" 
        style="display: block; margin: 0 auto"/>

이렇게 학습한 모델은 `context embedding`에 따라 다양한 이미지를 생성할 수 있다. 

<img src="assets/computer_vision/diffusion_model/how_diffusion_models_work/context_sampling.png" alt="Picture"
        width="700" 
        height="200" 
        style="display: block; margin: 0 auto"/>

> 학습에서 사용하지 않은 `context`에 대해서도 `context embedding`으로 위와 같이 합성된 새로운 이미지를 생성하는 것이 가능하다. 

***즉, `context embedding`을 통해서 생성할 이미지에 대한 제어가 가능하다!***

### Speeding Up

- **DDIM** is fater b/c it skips timesteps
- `Sampler`가 다르게 적용된 것으로 볼 수 있다.
    - 그렇기 때문에 기존의 **DDPM**과 거의 동일하게 학습/사용이 가능하다.

## References:
- [deeplaerning.ai - How Diffucion Models Work](https://www.deeplearning.ai/short-courses/how-diffusion-models-work/)