---
layout: post
title: Imitation Learning
category: Reinforcement Learning
tag: [rl, imitation learning]
---


# [Imitation Learning for RL](https://web.stanford.edu/class/cs237b/pdfs/lecture/lecture_10111213.pdf)

```
Imitation Learning
├── Behavior Cloning (BC)
├── Offline RL + Demonstrations (e.g., DQfD, SQIL)
├── Inverse Reinforcement Learning (IRL)
├── Adversarial Imitation Learning (AIL, e.g. GAIL)
├── Large-scale, Foundation Model 기반 IL
├── etc.
```


# How to apply Imitation Learning to RL?


## Behavior cloning

강화학습의 최종 목표는 주어진 task를 수행하기 위한 policy의 학습이다. 이 때, 처음부터 random으로 policy를 학습하기 보다는 정답지를 활용하여 policy를 학습하고, 이를 fine-tuning과 같이 general하게 강화학습으로 학습함으로서 학습을 개선함과 동시에 효율적인 학습이 가능하다. 

* 이 때, reward가 명시적으로 필요하지 않을 수도 있음 -> state에 따른 action이 정해져 있으므로 이에 대한 supervised learning (with loss function such as maximum likelihood esitimation)

> 학습된 policy는 imitation learning과 다른 state에 대해서는 성능 저하가 발생할 수 있으므로, **[DAgger(Dataset Aggregation)](https://www.cs.cmu.edu/~mgormley/courses/10418/slides/lecture5-l2s.pdf)** 적용


* [Overcoming Exploration in Reinforcement Learning with Demonstrations](https://arxiv.org/pdf/1709.10089)

* [Decision Transformer: Reinforcement Learning via Sequence Modeling](https://arxiv.org/abs/2106.01345)

* [Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/abs/2303.04137) (1000 citations)

## Offline RL + Demonstrations

* [Deep Q-learning from Demonstrations](https://arxiv.org/abs/1704.03732) (1,476 citations)

    * reward 사용 -> human demonstration에 대한 데이터를 모을 때, reward도 함께 모음

* [PIRLNav: Pretraining with Imitation and RL Finetuning for OBJECTNAV](https://arxiv.org/pdf/2301.07302) (90 citations)

* [Deep Reinforcement Learning for Robotic Manipulation with Asynchronous Off-Policy Updates](https://arxiv.org/pdf/1610.00633) (2100 citation)

* [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817) (930 citations)

* [SQIL: Imitation Learning via Reinforcement Learning with Sparse Rewards](https://arxiv.org/abs/1905.11108)

* [AWAC: Accelerating Online Reinforcement Learning with Offline Datasets](https://arxiv.org/abs/2006.093590)

* [SERL: A Software Suite for Sample-Efficient Robotic Reinforcement Learning](https://arxiv.org/pdf/2401.16013)

* [Precise and Dexterous Robotic Manipulation via Human-in-the-Loop Reinforcement Learning](https://arxiv.org/pdf/2410.21845)

* Integrating behavior cloning and reinforcement learning

    **Cycle of learning(CoL)** 프레임워크를 사용하여, 강화학습과 behavior cloning을 loss function으로 결합하여 동시에 학습하는 방식


    * [Integrating Behavior Cloning and Reinforcement Learning for Improved Performance in Sparse Reward Environments](https://arxiv.org/pdf/1910.04281v1)

    * [Integrating Behavior Cloning and Reinforcement Learning for Improved Performance in Dense and Sparse Reward Environments](https://www.ifaamas.org/Proceedings/aamas2020/pdfs/p465.pdf)



## Adversarial Imitation Learning

보상 함수 설계의 어려움을 해결하면서도 탐색 능력을 유지하는 방법


* **Discriminator**: 전문가 trajectory와 agent trajectory를 구분
* **Generator**: discriminator을 속이는 policy 학습
* 강화학습: discriminator의 output을 reward function에 활용하여 환경과 상호작용하며 학습


* [Integration of Imitation Learning using GAIL and Reinforcement Learning using Task-achievement Rewards via Probabilistic Graphical Model](https://arxiv.org/abs/1907.02140) (30 citations)

* [InfoGAIL: Interpretable Imitation Learning from Visual Demonstrations](https://papers.nips.cc/paper/2017/file/2cd4e8a2ce081c3d7c32c3cde4312ef7-Paper.pdf) (500 citations)

* [Goal-Aware Generative Adversarial Imitation Learning](https://arxiv.org/abs/2209.10149)


## Etc.

#### [RILe: Reinforced Imitation Learning](https://arxiv.org/abs/2406.08472)


#### [RLIF: INTERACTIVE IMITATION LEARNING AS REINFORCEMENT LEARNING](https://arxiv.org/pdf/2311.12996)



## Demonstration-focused from hugging face by `behavior cloning` keyword

#### [Behavior Transformers: Cloning k modes with one stone](https://arxiv.org/pdf/2206.11251)

#### [MimicGen: A Data Generation System for Scalable Robot Learning using Human Demonstrations](https://arxiv.org/pdf/2310.17596)

#### [Goal-Conditioned Imitation Learning using Score-based Diffusion Policies](https://arxiv.org/pdf/2304.02532)

#### [Coarse-to-fine Q-Network with Action Sequence for Data-Efficient Robot Learning](https://arxiv.org/pdf/2411.12155)

#### [Continuous Control with Coarse-to-fine Reinforcement Learning](https://arxiv.org/pdf/2407.07787)

## References

* [Deep Reinforcement Learning Bible](https://wikidocs.net/book/7888)
* [DAgger(Dataset Aggregation)](https://www.cs.cmu.edu/~mgormley/courses/10418/slides/lecture5-l2s.pdf)

