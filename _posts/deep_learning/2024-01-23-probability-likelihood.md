---
layout: post
title: Probability & Likelihood
category: Deep Learning
tag: likelihood
---

#### Probability
- 확률
- *주어진 확률 분포가 고정된 상태*에서, 관측되는 사건이 변할 때 확률을 표현
    - *선택 가능한 정수의 범위를 1~5로 제한(확률 분포를 고정)한 상태에서 관측 목표값이 1~5 중에 한개 숫자(관측 되는 사건이 변화)가 될 경우*, 확률에 대한 단어를 Probability로 표현 (이 경우 확률값은 0.2)

#### Likelihood
- 확률
- *관측된 사건이 고정된 상태*에서, 확률 분포가 변할 때 (=확률 분포를 모를 때 = 가정할 때) 확률을 표현
    - *선택 가능한 정수의 범위를 1~5가 아닌 다른 정수 범위 1~10 또는 4~50으로 바꾸면서(=확률 분포를 모름) 2가 관측될 확률을 계산(관측 사건이 고정) 할 경우*, 확률에 대한 단어를 Likelihood로 표현


$$
L(\theta | x) = P(x | \theta) = p_{\theta}(x) = P_{\theta}(X = x)
$$

- $L(\theta)


## References:
- [[개념 정리] Likelihood 와 Probability](https://xoft.tistory.com/30)