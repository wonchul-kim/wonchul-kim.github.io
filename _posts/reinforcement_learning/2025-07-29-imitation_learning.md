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


#### [Overcoming Exploration in Reinforcement Learning with Demonstrations](https://arxiv.org/pdf/1709.10089)

#### [Decision Transformer: Reinforcement Learning via Sequence Modeling](https://arxiv.org/abs/2106.01345)

#### [Diffusion Policy: Visuomotor Policy Learning via Action Diffusion](https://arxiv.org/abs/2303.04137) (1000 citations)

------------------------------------------------------------------
## Offline RL + Demonstrations

#### [Deep Q-learning from Demonstrations](https://arxiv.org/abs/1704.03732) (1,476 citations)

* reward 사용 -> human demonstration에 대한 데이터를 모을 때, reward도 함께 모음

#### [PIRLNav: Pretraining with Imitation and RL Finetuning for OBJECTNAV](https://arxiv.org/pdf/2301.07302) (90 citations)

#### [Deep Reinforcement Learning for Robotic Manipulation with Asynchronous Off-Policy Updates](https://arxiv.org/pdf/1610.00633) (2100 citation)

#### [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817) (930 citations)

* 기존의 **DDPG** 알고리즘을 기반으로 개선점과 함께

* `demonstration data`를 어떻게 사용해야 하는지에 대해서도 다루고 있음

    * **PER**에서 online exploration data와 demonstration data에 대한 개선안

    * `n-step return`을 critic network update에 적용


#### [SQIL: Imitation Learning via Reinforcement Learning with Sparse Rewards](https://arxiv.org/abs/1905.11108)

#### [AWAC: Accelerating Online Reinforcement Learning with Offline Datasets](https://arxiv.org/abs/2006.093590)

#### [SERL: A Software Suite for Sample-Efficient Robotic Reinforcement Learning](https://arxiv.org/pdf/2401.16013)

#### [Precise and Dexterous Robotic Manipulation via Human-in-the-Loop Reinforcement Learning](https://arxiv.org/pdf/2410.21845)

* Integrating behavior cloning and reinforcement learning

    **Cycle of learning(CoL)** 프레임워크를 사용하여, 강화학습과 behavior cloning을 loss function으로 결합하여 동시에 학습하는 방식


    #### [Integrating Behavior Cloning and Reinforcement Learning for Improved Performance in Sparse Reward Environments](https://arxiv.org/pdf/1910.04281v1)

    #### [Integrating Behavior Cloning and Reinforcement Learning for Improved Performance in Dense and Sparse Reward Environments](https://www.ifaamas.org/Proceedings/aamas2020/pdfs/p465.pdf)


------------------------------------------------------------------
## Adversarial Imitation Learning

보상 함수 설계의 어려움을 해결하면서도 탐색 능력을 유지하는 방법


* **Discriminator**: 전문가 trajectory와 agent trajectory를 구분
* **Generator**: discriminator을 속이는 policy 학습
* 강화학습: discriminator의 output을 reward function에 활용하여 환경과 상호작용하며 학습


#### [Integration of Imitation Learning using GAIL and Reinforcement Learning using Task-achievement Rewards via Probabilistic Graphical Model](https://arxiv.org/abs/1907.02140) (30 citations)

#### [InfoGAIL: Interpretable Imitation Learning from Visual Demonstrations](https://papers.nips.cc/paper/2017/file/2cd4e8a2ce081c3d7c32c3cde4312ef7-Paper.pdf) (500 citations)

#### [Goal-Aware Generative Adversarial Imitation Learning](https://arxiv.org/abs/2209.10149)

------------------------------------------------------------------
## Etc.

#### [RILe: Reinforced Imitation Learning](https://arxiv.org/abs/2406.08472)


#### [RLIF: INTERACTIVE IMITATION LEARNING AS REINFORCEMENT LEARNING](https://arxiv.org/pdf/2311.12996)


#### [Visuomotor Policy Learning via Action Diffusion](https://diffusion-policy.cs.columbia.edu/)


------------------------------------------------------------------
## Demonstration-focused from hugging face by `behavior cloning` keyword

#### [Behavior Transformers: Cloning k modes with one stone](https://arxiv.org/pdf/2206.11251)

#### [MimicGen: A Data Generation System for Scalable Robot Learning using Human Demonstrations](https://arxiv.org/pdf/2310.17596)


#### [Goal-Conditioned Imitation Learning using Score-based Diffusion Policies](https://arxiv.org/pdf/2304.02532)

#### [Coarse-to-fine Q-Network with Action Sequence for Data-Efficient Robot Learning](https://arxiv.org/pdf/2411.12155)

#### [Continuous Control with Coarse-to-fine Reinforcement Learning](https://arxiv.org/pdf/2407.07787)

* **imitation learning**이 주제는 아니고, `demonstration data`를 사용함으로서의 효율성을 보여줄 뿐

* value-based RL 알고리즘으로 continuous action space의 task를 풀 수 있다는 점과 함께 기존방식의 개선점을 도출

    * multiple-level & n-discretization (기존에는 1-level multiple discretization)


------------------------------------------------------------------
## 3D 

#### [RVT-2: Learning Precise Manipulation from Few Demonstratio](https://arxiv.org/pdf/2406.08545)

#### [SPOT: SE(3) Pose Trajectory Diffusion for Object-Centric Manipulation](https://arxiv.org/pdf/2411.00965)

## References

* [Deep Reinforcement Learning Bible](https://wikidocs.net/book/7888)
* [DAgger(Dataset Aggregation)](https://www.cs.cmu.edu/~mgormley/courses/10418/slides/lecture5-l2s.pdf)


----------------------------------------------------------------------

현재 로봇팔의 Pick-and-Place 같은 복잡한 조작 작업에서 보고된 SOTA(성공률)는 크게 두 축으로 나뉩니다.

1. 순수 RL 기반 SOTA: SERL / HIL-SERL
– “Sample-Efficient Robotic RL (SERL)” 프레임워크는

프랭카(Franka) 팔을 이용한 PCB 삽입, 케이블 라우팅, 오브젝트 재배치 등에서

15–60분의 실제 환경 학습 만으로 거의 완벽한(≈100%) 성공률을 기록했습니다.
– HIL-SERL(“Human-in-the-Loop SERL”)은

사람의 시연 및 교정 데이터를 함께 사용해

1–2.5시간 내에 100% 성공률을 달성하는 것을 보여주었습니다.

2. 대규모 사전학습 VLA/언어–비전–행동 모델
– RT-Series (RT-1, RT-2, RT-H): 인터넷 스케일의 웹·로봇 데이터로 학습,

복잡한 pick-and-place 작업에서 80–90% 이상의 성공률.
– RoboCat: Decision Transformer 기반,

단일 데모로도 pick-and-place를 90% 이상 달성.

3. Diffusion 기반 정책
– Time-Unified Diffusion Policy (TUDP):

RLBench pick-and-place 태스크에서 82.6–83.8% 성공률.
– DreamGen: 4단계 파이프라인,

pick-and-place 포함 다중 조작 태스크에서 85–90% 성공률.

4. Transformer 및 토폴로지 기반 모델
– M2T2 (Multi-Task Masked Transformer):

RLBench pick-and-place에서 75–85% 성공률 달성.
– PointFlowMatch (포인트 클라우드 + Flow Matching):

평균≈68% 성공률, 다른 SOTA 대비 2배 성능.


요약하면,
100% 성공률이 보고된 것은 SERL/HIL-SERL 계열(실제 로봇, 인간교정 포함)이며,
순수 RLBench·시뮬레이션 환경에서는 Diffusion, VLA, Transformer 기반 기법들이 80–90% 수준으로 SOTA입니다.

> 경쟁 논문들

# Pick-and-Place Task SOTA 기법 비교

로봇팔의 Pick-and-Place 같은 복잡한 manipulation task에서 **100% 성공률**을 달성한 주요 SOTA 기법들입니다.

| 알고리즘    | 성공률   | 학습시간               | 주요 특징                                                             |
|-----------|--------|----------------------|---------------------------------------------------------------------|
| **SERL**     | 100%   | 30–60분              | 데모 데이터만 사용, 실시간 인간 개입 없음                                    |
| **HIL-SERL** | 100%   | 1–2.5시간            | 데모 + 실시간 인간 교정, Teleoperation 기반 데이터 수집                        |
| **WSRL**     | 100%   | 18분                 | SERL이 실패한 태스크에서도 빠르게 100% 성공 달성                                 |
| **DYNA-1**   | 99.9%  | 24시간 연속           | 상업적 배포용 로봇 파운데이션 모델, 800+ 냅킨 접기 실험, 무인 운영                 |
| **ConRFT**   | 96.3%  | —                    | VLA(vision-language-action) 모델 파인튜닝, 다양한 pick-and-place 태스크에 적용      |
| **SkillGen** | 95–100%| —                    | 소수의 실제 시연으로 대규모 데모 자동 생성, zero-shot 일반화 지원                  |

## 각 기법 개요

1. WSRL (2025년 7월) 
100% 성공률: Franka peg insertion task에서 18분 내에 100% 달성

SERL 실패: 동일 태스크에서 SERL은 50분 동안 0/20 (0%) 성공률

UC Berkeley: 온라인 RL 파인튜닝 방법으로 오프라인 데이터 보존 없이 학습

2. Dyna Robotics DYNA-1 (2025년) 
99.9% 성공률: 3일 연속, 하루 8시간씩 실행하여 단 1개 수건만 떨어뜨림

99.4% 성공률: 24시간 동안 800+ 냅킨 접기 (인간 개입 없음)

60% 인간 처리량: 실제 상업적 배포 수준의 성능

Zero-shot 일반화: 다른 환경에서도 바로 작동

3. ConRFT (2025년 2월) 
96.3% 평균 성공률: 다양한 태스크에서 평균 성과

HIL-SERL 대비: 31.9% 대비 3배 향상

VLA 모델 파인튜닝: Consistency Policy를 통한 강화 파인튜닝

4. Figure Helix (2025년 6월) 
~95% 정확도: 바코드 방향 정렬에서 달성

4.05초/패키지: 빠른 처리 속도

메모리 통합: 장기 태스크를 위한 메모리와 힘 피드백

5. SkillGen (2024년) 
95% 성공률: Pick-Place-Milk task에서 달성

100% 성공률: Coffee D2 task에서 달성

3개 데모만으로: 100개 데모 자동 생성

6. 기타 100% 달성 사례들
QT-Opt (Google, 2018): 96% 그래스핑 성공률 

Bi-Manual RL: 100% 블록 픽업, 65% 연결 태스크 

ObjectVLA (2025): 100% ID 평가 성공률 

SRIL: 100-200% 속도 향상과 높은 성공률 


결론
SERL/HIL-SERL은 확실히 뛰어난 성과를 보이지만, 독점적인 100% 달성자는 아닙니다. 특히:

WSRL: SERL이 실패한 태스크에서 더 빠르게 100% 달성

DYNA-1: 더 긴 지속성과 상업적 실용성 입증

ConRFT: 더 높은 일반화 성능

따라서 현재 로봇팔 manipulation에서 100% 성공률을 달성한 SOTA 기법들이 여러 개 존재하며, 각각 서로 다른 강점과 접근법을 가지고 있습니다.



* https://github.com/philfung/awesome-reliable-robotics