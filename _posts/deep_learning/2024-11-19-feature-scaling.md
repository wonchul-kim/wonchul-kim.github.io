---
layout: post
title: Feature Scaling
category: Deep Learning
tag: [standardization, normalization]
---

## Normalization (정규화)

데이터셋에는 기준 또는 구분이 되는 feature가 존재하며, 각 feature에 대한 numerical value 범위에 대한 차이를 반영하지 않도록 공통의 범위로 변경하는 것.

<img src='/assets/deep_learning/feature_scaling/table.png'>

예를 들어, 위와 같이 각각의 `class`에 대해서 `Alcohol`과 `Malic acid`라는 feature가 있을 때, 각각의 feature에 대한 범위가 다르다. 

이 때, 각각의 중요도에 대해서 판단을 하면서 학습을 하게 될 경우, 크기에 의해서 중요도가 판단될 수 있기에 학습이 되지 않을 수 있다. 

그렇기 때문에 **normalization**은 각각의 feature vector를 euclidian distance가 1이 되도록 한다. 즉, 지름이 1인 원 또는 구에 모든 데이터를 projection한다.(각각의 feature에 대해서 scaling ratio를 달리하여 동일한 범위 내로 조정하는 것)

> 이는 feature vector의 길이는 상관이 없고, 데이터의 방향만이 중요한 것이라고 할 수 있다. 


##### Min-Max scaling

$$ 
x' = \frac{x - x_{min}}{x_{max} - x_{min}}
$$


## Standardization

표준정규분포의 속성($\mu=0, \sigma=1$)을 갖도록 feature의 값이 조정된다. 

$$
z = \frac{x - \mu}{\sigma}
$$

- feature의 크기에 대해서 최대/최소값을 제한하지 않기 때문에 outlier를 파악할 수도 있다. 


## Normalization vs. Standardization

<img src='/assets/deep_learning/feature_scaling/versus.png'>


일반적으로 standardizatoin을 통해서 outlier를 제거하고, normalization을 통해서 최소/최대값을 제한한다. 

> In addition, we’d also want to think about whether we want to “standardize” or “normalize” (here: scaling to [0, 1] range) our data. Some algorithms assume that our data is centered at 0. For example, if we initialize the weights of a small multi-layer perceptron with tanh activation units to 0 or small random values centered around zero, we want to update the model weights “equally.” As a rule of thumb I’d say: When in doubt, just standardize the data, it shouldn’t hurt.


### references:
- [About Feature Scaling and Normalization](https://sebastianraschka.com/Articles/2014_about_feature_scaling.html#about-standardization)

- https://heeya-stupidbutstudying.tistory.com/entry/%ED%86%B5%EA%B3%84-%EC%A0%95%EA%B7%9C%ED%99%94%EC%99%80-%ED%91%9C%EC%A4%80%ED%99%94