---
layout: post
title: Few-shot Learning
category: Few-shot Learning
tag: [few-shot learning]
---

# Few-shot Learning

- Meta Learning의 한 종류
    - “배우는 법을 배우”려면 많은 데이터가 필요
    - 다른 점은 구분하려는 문제의 데이터는 Training set에 없어도 된다.

- 헷갈리지 말아야 할 것은 “Few”한 데이터로 학습을 한다는 의미는 아니라 적은 데이터로 추론한다는 것
    - `Training set` 통해 “구분하는 법을 배우”고, `Query image`가 들어왔을 때 이 `Query image`가 `Support set` 중 어떤 것과 같은 종류인지를 맞추는 일을 하는 것

- `Query image`가 어떤 클래스에 “속하느냐”의 문제를 푸는 것이 아니라 어떤 클래스와 “같은 클래스냐”의 문제를 푼다고 생각

- `Training set`, `Support set`, `Query image`가 필요

- 기본 학습 방법은 ***유사성(similarity)***을 학습하는 것

    - `dataset`: 은 `postive samples`과 `negative samples`로 구성
    - train: `positive samples`끼리는 비슷한 feature가, `negative samples`끼리는 비슷하지 않은 feature가 추출되도록 `positive`와 `negative`를 번갈아 가며 학습
    - prediction: support set의 이미지와 query image의 유사성을 검사

### Support set

- k-way: support set을 구성하는 class의 갯수
    - `query image`가 $k$개의 class 중 어떤 것과 같은지 묻는 것
    - $k$는 성능과 반비례

- n-shot: 각 class가 가진 sample 갯수
    - $n$은 성능과 비례
    - $n = 1$일 때, **one-shot learning**


### Types

#### 1. Metric-based

- **Siamese Network** 구조를 통해서 Similarity를 측정할 수 있도록 학습

- 이렇게 하면, `unseed data`에 대해서도 구분 가능

#### 2. Optimization-based

- `Base Network`: 실제 task에 있어 학습
- `Meta Network`: `Base Network`를 optimization

- `Learning to learn gradient descent by gradient descent`
- `Model-agnostic Meta-learning`

#### 3. Model-based

- `Memory-augmented Neural Networks`

## References:
- https://www.youtube.com/watch?v=hE7eGew4eeg