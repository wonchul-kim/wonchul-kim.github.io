---
layout: post
title: How to improve GPUs capacity and training time
category: Pytorch
tag: torch
---

# To REDUCE GPU capacity and make training FASTER

## Mixed Precision


## XLA

- TPU에서 최적화될 수 있기 때문에 GPU에서는 단점이 더 많을 수 있음
-----------------------------------------------------------------------------

## `torch.compile()`

- 모델에 대해서 최적화를 진행함
- 추론속도가 빨라질 수 있으나, 오히려 VRAM이 증가할 수 있는 단점이 존재
-----------------------------------------------------------------------------

## Gradient Checkpointing

- VRAM 사용량을 줄이는데에 있어서는 매우 효과적! 
    - **sementation**: `encoder(backbone)`, `decoder`, `head`에 적용할 수 있으며, 복잡하고 연산이 많을 수록 효과적
        - `encoder`에 대해서 40%정도 VRAM이 줄어듬

- 계산하는 시간이 늘어난다는 단점이 있지만, trade-off를 고려해서 어떤 layer에 적용할지 잘 선택할 수 있다는 점에서 어떻게 적용하느냐가 중요!!!

- 사용법은 매우 단순하다
    ```python
    from torch.utils.checkpoint import checkpoint 

    x = checkpoint(layer, input)
    ```

    - layer: 연산 결과를 저장하지 않을 layer 
    - input: 해당 layer의 input


- [Fitting larger networks into memory](https://medium.com/tensorflow/fitting-larger-networks-into-memory-583e3c758ff9)

- [Training Deep Nets with Sublinear Memory Cost](https://arxiv.org/pdf/1604.06174v2)
-----------------------------------------------------------------------------

## PyTorch Native CUDA 연산 


-----------------------------------------------------------------------------

## Scaled Dot Product Attention (SDPA)



- https://tutorials.pytorch.kr/intermediate/scaled_dot_product_attention_tutorial.html
-----------------------------------------------------------------------------


## Flash Attention


-----------------------------------------------------------------------------


## References

- [Accelerating Large Language Models with Accelerated Transformers](https://pytorch.org/blog/accelerating-large-language-models/)

- https://pytorch.kr/blog/2023/accelerating-large-language-models/