---
layout: post
title: Diffusion Policy
category: Reinforcement Learning
tag: [rl, imitation learning, diffusion policy]
---

# [Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/pdf/2303.04137)

## Abs.

* a new way of generating robot behavior by representing a robot’s visuomotor policy as a conditional denoising diffusion process. 

* Diffusion Policy learns the gradient of the action-distribution score function and iteratively optimizes with respect to this gradient field during inference via a series of stochastic Langevin dynamics steps. 

    * We find that the diffusion formulation yields powerful advantages when used for robot policies, including gracefully handling multimodal action distributions, being suitable for high-dimensional action spaces, and exhibiting impressive training stability. 
    
    * a set of key technical contributions including the incorporation of receding horizon control, visual conditioning, and the time-series diffusion transformer. 
    

## Intro.

* Policy learning from demonstration, in its simplest form, can be formulated as the supervised regression task of learning to map observations to actions. 

    * In practice however, the unique nature of predicting robot actions — such as the existence of multimodal distributions, sequential correlation, and the requirement of high precision — makes this task distinct and challenging compared to other supervised learning problems. 

* In diffusion policy, instead of directly outputting an action, the policy infers the action-score gradient, conditioned on visual observations, for K denoising iterations. 

* Expressing multimodal action distributions. 

    * By learning the gradient of the action score function Song and Ermon (2019) and performing Stochastic Langevin Dynamics sampling on this gradient field, Diffusion policy can express arbitrary normalizable distributions Neal et al. (2011), which includes multimodal action distributions, a well-known challenge for policy learning.

* High-dimensional output space. 

    * diffusion models have shown excellent scalability to highdimension output spaces. This property allows the policy to jointly infer a sequence of future actions instead of single-step actions, which is critical for encouraging temporal action consistency and avoiding myopic planning.

* Stable training. 
    
    * Training energy-based policies often requires negative sampling to estimate an intractable normalization constant, which is known to cause training instability Du et al. (2020); Florence et al. (2021). Diffusion Policy bypasses this requirement by learning the gradient of the energy function and thereby achieves stable training while maintaining distributional expressivity.




1. **Closed-loop action sequences**
    
    * We combine the policy’s capability to predict high-dimensional action sequences with receding-horizon control to achieve robust execution. This design allows the policy to continuously re-plan its action in a closed-loop manner while maintaining temporal action consistency – achieving a balance between long-horizon planning and responsiveness.

2. **Visual conditioning**

    * We introduce a visionconditioned diffusion policy, where the visual observations are treated as conditioning instead of a part of the joint data distribution. In this formulation, the policy extracts the visual representation once regardless of the denoising iterations, which drastically reduces the computation and enables real-time action inference.

3. **Time-series diffusion transformer**

    * We propose a new transformer-based diffusion network that minimizes the over-smoothing effects of typical CNN-based models and achieves state-of-the-art performance on tasks that require high-frequency action changes and velocity control.


## Diffusion Policy Formulation

Diffusion policies are able to express complex multimodal action distributions and possess stable training behavior – requiring little task-specific hyperparameter tuning. 

### Denosing Diffusion Probabilistic Models

DDPMs are a class of generative model where the output generation is modeled as a denoising process, often called
Stochastic Langevin Dynamics Welling and Teh (2011).

## References
 
- [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817)