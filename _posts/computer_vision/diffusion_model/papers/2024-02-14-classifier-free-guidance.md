---
layout: post
title: CLASSIFIER-FREE DIFFUSION GUIDANCE
category: Computer Vision
tag: [diffusion model, ddpm]
---

# [CLASSIFIER-FREE DIFFUSION GUIDANCE](https://arxiv.org/pdf/2207.12598.pdf)

**Classfier Guidance**의 `classifier`의 존재에 의한 단점을 해결하고자, `classifier`없이 conditional sampling을 가능하게 함


#### Classifier Free Guidance 
- **Classfier Guidance**와 많이 다르지 않으며, 수식을 다르게 전개했을 뿐
    $$
    \nabla_y logp(\mathbf{x_t}|y) = \nabla_y logp(\mathbf{x_t}) + \nabla_y logp(y|\mathbf{x_t}) \\
    \nabla_y logp(y|\mathbf{x_t}) = \nabla_y logp(\mathbf{x_t}|y) - \nabla_y logp(\mathbf{x_t})
    $$

    ...
    
    - 모델에 직접적으로 condition을 넣어주면 성능이 나오지 않지만, $\gamma$을 잘 조절함으로서 성능을 개선할 수 있다. 


### References:
- [CLASSIFIER-FREE DIFFUSION GUIDANCE](https://arxiv.org/pdf/2207.12598.pdf)
- https://sander.ai/2022/05/26/guidance.html