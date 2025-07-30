---
layout: post
title: Continuous Control with Coarse-to-fine Reinforcement Learning
category: Reinforcement Learning
tag: [rl, crl]
---

# [Continuous Control with Coarse-to-fine Reinforcement Learning](https://arxiv.org/pdf/2407.07787)

#### Coarse-to-fine Reinforcement Learning (CRL)

a framework that trains RL agents to zoom-into a continuous action space in a coarse-to-fine manner, enabling the use
of stable, sample-efficient value-based RL algorithms for fine-grained continuous control tasks.

* Iterates the procedure of 
    * discretizing the continuous action space into multiple intervals and
    * selecting the intervals with the highest Q-value to further discretize at the next level
* Coarse-to-fine Q-Network (CQN)
* robustly learns to solve real-world manipulation tasks within a few minutes of online training


## Introduction

* Continuous control domain에 있어서 Actor-critic algorithm이 기반을 잡았지만, actor와 critic network가 서로 복잡한 관여하는 부분이 많이짐에 따라 불안정한 학습을 보임

* 개념적으로 value-based RL algorithm은 critic network만 사용하는 것이기 때문에 더 안정적으로 학습이 가능

    * 하지만, action space가 discrete이기 때문에 단순 적용은 불가능

    * continuous action space를 여러 interval로 이산화하면 value-based RL algorithm이 적용될 수 있다는 연구가 진행됨

    * 이 때, discretization scheme에 따라 precision of actions and sample-efficiency간의 trade-off가 문제가 됨

        * 더 세밀한 이산화일수록 정확하지만 데이터가 많아짐



## Method

*  기존의 **CRL**은 single level에서 여러 bins로 이산화를 진행했지만, 본 논문에서는 여러 레벨에서 단계적으로 bin의 갯수를 늘리면서 진행


#### Inputs and ecoder

* **Inputs**: $\mathbf{o}_t$ consisting of
    * pixel observations $(\mathbf{o}_t^{v_1}, ..., \mathbf{o}_t^{v_V} )$ captured from viewpoints $(v_1, ..., v_V)$ 
    * low-dimensional proprioceptive states $\mathbf{o}_t^{\text{low}}$. 
    
* **Encoder**: We then use a lightweight 4-layer convolutional neural network (CNN) encoder $f_\theta^{\text{enc}}$ to encode pixels $\mathbf{o}_t^{v_i}$ into visual features $\mathbf{h}_t^{v_i}$, i.e., $\mathbf{h}_t^{v_i} = f_\theta^{\text{enc}}(\mathbf{o}_t^{v_i})$. To fuse information from view-wise features, we concatenate features from all viewpoints and project them into low-dimensional features. Then we concatenate fused features with proprioceptive states $\mathbf{o}_t^{\text{low}}$ to construct features $\mathbf{h}_t$.