---
layout: post
title: FlashAttention
category: Deep Learning
tag: [flash attention]
---

# FlashAttention (Fast and Memory-Efficient Exact Attention with IO-Awareness)

<img src='/assets/deep_learning/flash_attention/flash_attention.png'>

* 기존의 transformer의 attention 계산에는 memory-access에 관련된 비효율이 존재

* GPU에서 연산할 때, HBM(Main memory of GPU)으로의 접근이 너무 많아 I/O communication이 bottleneck이 되고 있음

* 즉, Load/Write 동작을 압도적으로 줄였고, SRAM(register? cache of GPU)에서 한 번에 computation을 진행



