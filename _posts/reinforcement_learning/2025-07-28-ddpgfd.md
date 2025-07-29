---
layout: post
title: DDPGfD
category: Reinforcement Learning
tag: [rl, imitation learning, ddpgfd]
---

# [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817)

- AAAI 2017
- 약 1000회 citations

### How it works

- Base 알고리즘: **DDPG (Deep Deterministic Policy Gradient)**
    - with **Prioritized Experience Replay**

- Modifications
    1. PER에는 demonstration data가 존재하며, exploration을 통한 data와 통합하여 사용하되, 기존의 prioritization에 demo. vs. exploration 에 대한 weighting을 고려
    2. `n-step return`이 critic network에 대한 loss를 추가
    3. multiple learning updates per environment stop

### Experiments (`before training` vs. `after training`)

- env: **gymnasium & gymnasium_robotics**
- after 50 episodes

<img src='/assets/reinforcement_learning/ddpgfd/before_training.gif' alt='before training'>
<img src='/assets/reinforcement_learning/ddpgfd/after_training.gif' alt='after training'>

- 학습하기 이전에는 당연히 random action이기 때문에 아무렇게 움직임
- Demonstration으로 pretrain하고 fine-tuning을 진행하면 policy가 더 빠르게 수렴함
- 즉, demonstration을 통해서 효율성을 달성


## References
 
- [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817)