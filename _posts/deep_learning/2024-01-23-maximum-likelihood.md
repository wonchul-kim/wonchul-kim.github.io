---
layout: post
title: Maximum Likelihood Estimation (MLE)
category: Deep Learning
tag: likelihood
---

- **Likelihood**는 확률분포가 가정된 상황에서 관측값이 가지는 확률을 의미한다. 

- **Maximum Likelhood Estimation (MLE)**는 관측되는 데이터들을 가장 잘 모델링하는 확률분포의 `parameter`를 찾는 알고리즘이다. 


### 확률분포

- 관측값($x$)를 입력으로 확률값($y$)을 출력하는 확률 밀도 함수(pdf)로 정의된다.
- parameter($\theta$)를 변수로 가지고 있을 때, peak은 1개이다. 즉, 미분했을 때, 0이 되는 지점은 1개이다. 

#### Exponential Distribution

$$ 
y = \lambda e^{-\lambda x}
$$

- $\lambda$: **exponential distribution**에 대한 parameter 


#### Gaussian[Normal] Distribution 

$$
p(x|\mu, \sigma) = \frac{1}{\sqrt{2\pi \sigma^2}}e^{-\frac{(x - \mu)^2}{2\sigma^2}}
$$



## References:
- [[개념 정리] Maximum Likelihood Estimation 와 Log Likelihood](https://xoft.tistory.com/31)