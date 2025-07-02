---
layout: post
title: To Reduce Dataset Size
category: Deep Learning
tag: [reduce dataset]
---

데이터와 모델은 크기가 클수록 좋다는 **scaling law**를 따른다는 것은 이미 여러 연구로부터 검증이 되었지만, computation과 학습시간을 고려했을 때에는 매우 비효율적일 수 있다.

특히, 테이터의 관점에서 수 많은 데이터가 있음에도 서로 비슷한 데이터가 존재하거나 모델의 성능에 영향을 미치지 못하는 등의 학습에 불필요한 (제거해도 상관없는) 데이터가 존재할 수 있다. 아마도 대용량의 데이터일수록 더욱 더 그럴 것이다. 이러한 데이터는 오히려 학습 성능에 편향을 일으킬 수도 있기 때문에 제거하는 것이 좋을 것이 좋지 않을까 한다!

즉, 제거함으로서 모델의 성능, computation, 학습 시간에 모두 이득이 되지 않을까!?


이를 위한 연구의 출발로서 전제는 일단 다음과 같이 2가지로 나뉠 수 있다. 

1. 레이블링된 데이터셋이 이미 존재하고, 이에 대한 데이터셋 정리의 관점에서 시작 
    - **Data pruning**: 말 그대로 데이터셋을 제거하는 연구
    - **Dataset distillation**: 대용량의 데이터셋의 정보를 중/소용량의 데이터셋에 모두 담고자 하는 연구
    - **Data curation**: 데이터 정제??? high quality 데이터 남기기 위해? 만들기 위해?
    - **Clustering**: 중요한 / 중요하지 않은 데이터를 나누는 연구


    * 학습하는 동안 중요하지 않은 데이터는 학습에서 제외

        - [InfoBatch]

        - [Deep Learning on a Healthy Data Diet: Finding Important Examples for Fairness, AAAI2023](https://arxiv.org/pdf/2211.11109)

        - [FoMo-0D: A Foundation Model for Zero-shot Outlier Detection](https://arxiv.org/pdf/2409.05672)

        - [Deep Learning on a Data Diet: Finding Important Examples Early in Training](https://arxiv.org/pdf/2107.07075)

        - [Continual Learning on a Data Diet](https://arxiv.org/pdf/2410.17715)

2. 레이블링이 아직 안성되지 않았고, 레이블링이 되지 않은 데이터셋에 대해서 human annotation을 줄이고자 함
    - **Active learning**: 최소의 레이블링으로 최대의 성능을 내자는 연구
    - **Self-supervised learning**: 레이블링이 되지 않은 데이터를 사용하는 관점에서 사용되는 듯






