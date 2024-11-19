---
layout: post
title: Layer Normalization
category: Deep Learning
tag: [layer normalization]
---

### Batch Normalization 
(normalization이라고 하지만, 오히려 standardization에 더 가깝다!)

- 각 feature별로 하나의 mini-batch에 대한 summed input의 평균과 분산을 구해서 normalization을 진행

- fully-connected layer: 
    - $x$: N x D
    - $\mu, \sigma, \gamma, \beta$: 1 x D

- convolutional layer: 
    - $x$: N x C x H x W
    - $\mu, \sigma, \gamma, \beta$: 1 x C x 1 x 1

- model layer의 계층이 쌓여있을 때, 각 층의 activation layer를 통과하기 전에 batch normalization을 진행

- 장점: 
    - generalization 개선하여 overfitting 감소
        - mini-batch에서 feature를 normaliztion하기 때문에 네트워크가 각 층에서의 입력 분포에 덜 민감해지고(네트워크가 특정 특성에 집중하지 않게 됨), 이는 훈련 데이터에 과도하게 overfit을 방지한다.
    - 학습의 안정성 및 수렴속도 개선
    - 가중치 초기화에 강건
        - 각 레이어의 입력 분포가 일정하게 유지되므로 초기 가중치가 크거나 작더라도 학습이 잘 이루어지도록 한다. 
        - activation 함수가 초기에 너무 큰 값이나 작은 값을 갖는 것을 방지하여 학습이 안정적으로 진행될 수 있도록 한다.
    - 기울기 소실 문제 완화
        - activation 함수가 너무 작은 값을 갖는 것을 방지한다.
        - 더 큰 학습률 사용 가능: 네트워크가 더 넓은 범위의 학습률을 사용하면서도 안정적으로 학습할 수 있다. 이는 기울기 소실 문제를 더욱 효과적으로 방지하는 요소이다.
        - 출력의 분포를 일정하게 유지: 각 층에서 출력 분포가 일정하게 유지되므로, 역전파 중에 전달되는 기울기가 소실되는 현상이 줄어든다.
    - 피드포워드 네트워크 모델(input - hidden - output 순서로 한방향으로 흐르는 인공신경망 모델, mnist 0~9 손글씨 분류 등)에는 성능과 안정성 개선에 뛰어나다


- 단점:
    - batch size가 너무 작은 경우에는 잘 동작하지 않는다. (분산이 0이 되므로)
    - sequence data에 적용하기 힘들다. -> **layer normalization**을 적용


### [Layer Normalization](https://arxiv.org/abs/1607.06450)

- batch normalization에서의 batch에 대한 의존도를 제거하고, batch가 아닌 layer를 기준으로 normalization을 수행

- 하나의 layer의 모든 뉴런에 대한 summed input의 평균과 분산을 구한다. 

- fully-connected layer: 
    - $x$: N x D
    - $\mu, \sigma, \gamma, \beta$: N x 1

- convolutional layer: 
    - $x$: N x C x H x W
    - $\mu, \sigma, \gamma, \beta$: N x 1 x 1 x 1

- 장점
    - **Batch Normalization**은 작은 배치 크기에서 극단적 결과를 내는데 반해, 작은 batch size에서도 효과적인 이용이 가능
    - sequence에 따른 고정길이 정규화로 **batch normalization**에 비해 RNN 모델에 더 효과적인 방법이다.
    - 일반화 성능 향상이 가능하다
 
- 단점
    - 추가 계산 및 메모리 오버헤디가 발생할 수 있다
    - 반대로 피드포워드 네트워크 모델(input - hidden - output 순서로 한방향으로 흐르는 인공신경망 모델, mnist 0~9 손글씨 분류 등)에서는 **Batch Normalization** 만큼 잘 동작하지 않을 수 있다.
    - learning_rate, 가중치 초기화 같은 하이퍼파라미터 조정에 민감할 수 있다.


## References
- [Layer Normalization](https://arxiv.org/abs/1607.06450)
- http://dmqm.korea.ac.kr/activity/seminar/364