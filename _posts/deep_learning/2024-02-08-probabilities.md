---
layout: post
title: Probabilities
category: Deep Learning
tag: [probabilities]
---

### Probability Mass Function of Discrete Random Variable 

1. 모든 $x$에 대하여 $f(x) \ge 0$
2. $\sum^{\infty}_{i=1} f(x_i) = 1$
3. $P(a \le X \le b) = \sum_{a \le x_i \le b}f(x_i)$

### Joint Probability Distribution of Discrete Random Variable

두 개의 **이산확률변수**인 `X`와 `Y`에 대해서 각각 $x_1, x_2, ...$와 $y_1, y_2, ...$의 값을 가질 때, $P(X = x, Y = y)$를 만족하는 $f(x, y)$를 이산확률변수 `X`와 `Y`의 **결합확률분포**[**결확확률질량함수**]라고 한다. 

1. $f(x, y) \ge 0$
2. $\sum_{x} \sum_{y} f(x, y) = 1
3. $x, y$ 평면 상의 어떤 영역 $A$에 대해서, $P[(x, y) \in A] = \sum \sum_{A} f(x, y)$

### Marginal Probability Distribution 

결합확률분포 $f(x, y)$가 확률변수 $X$ 또는 $Y$만의 분포이면, 

(1) 확률변수가 이산확률변수일 경우, 
$$f_{X}(x) = \sum_{y}f(x, y) $$
$$f_{Y}(y) = \sum_{x}f(x, y)$$

(2) 확률변수가 연속확률변수일 경우,
$$f_{X}(x) = \int_{\infty}^{\infty}f(x, y)$$
$$f_{Y}(y) = \int_{\infty}^{\infty}f(x, y)$$

이 때, 확률변수 $X$와 $Y$가 서로 독립이면, 
$$f(x,y) = f_{X}(x)f_{Y}(y)$$

## References:
- [Probability Mass Function of Discrete Random Variable](https://blog.naver.com/mykepzzang/220835372463)
- [Joint Probability Distribution of Discrete Random Variable](https://blog.naver.com/mykepzzang/220836609004)
- [Marginal Probability Distribution](https://blog.naver.com/mykepzzang/220837645914)
