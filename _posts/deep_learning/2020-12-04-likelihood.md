---
layout: post
title: Likelihood
category: Deep Learning
tag: likelihood
---

# Maximum Likelihood (120분)

## 1. likelihood

 먼저 **likelihood**라는 것에 대한 개념이 약간 생소할 수 있지만, 이는 probability와 매우 유사한 개념이라고 할 수 있습니다. 둘 다 어떤 확률에 대한 것을 의미하지만, probability는 확률분포를 알고 있는 대상의 시행 전 확률을 의미하는 개념입니다. 그리고 likelihood는 확률분포를 모르는(정확히는 parameter) 대상의 관측된 데이터를 바탕으로 확률을 역추적할 때 쓰이는 개념이라고 생각하면 됩니다. 


 예를 들어, 동전을 던지는 시행에 있어서 앞면과 뒷면이 나올 확률은 각각 $\frac{1}{2}$입니다. 이러한 동전을 n번을 던진다고 할때, 기댓값은 $n*\frac{1}{2}$으로 계산할 수 있습니다. 그리고 이러한 계산법을 probability theory(확률론)의 접근방법이라고 할 수 있습니다. 하지만, 우리가 실제로 우리가 동전 던지기를 500번 수행하고 그 결과를 다음과 같이 기록하였습니다. 총 500번 중에서 앞면이 260번 나오고, 뒷면이 240번이 나왔습니다. 이러한 시행결과를 낳게 한 확률분포는 $p(앞면) = \frac{260}{500}$으로 계산될 것입니다. 그리고 이러한 $p$를 likelihood로 볼 수 있으며, 관측된 표본에 의해서 추정되기 떄문에 총합이 1이 아닐 수도 있습니다.
 
 앞서 말씀드렸듯이, likelihood는 어떠한 대상의 관측된 데이터를 바탕으로 확률분포를 만들 때, 이 확률분포에 사용되는 parameter들을 변화시킴으로서 역추적하는 것을 의미합니다. 그러므로 maximum likelihood는 이러한 확률분포를 최대로하는 parameter들을 찾는 것이라고 할 수 있습니다. 그리고 이러한 과정은 deep learning에서는 deep neural network의 parameter들을 찾는 과정과 매우 유사하다고 할 수 있습니다. 
 
<a href="https://imgur.com/7o1HLRT"><img src="https://imgur.com/7o1HLRT.png" width="600px" title="source: imgur.com" /></a>

즉, 위의 그림과 같이 주어진 데이터를 보고 이를 나타낼 수 있는 모델을 만드는 것이며, 모델은 자신의 parameter(보통은 $\theta$라고 지칭합니다.)들을 최적화하는 과정입니다. 

## 2. Probability density estimation

**density estimation**은 어떠한 대상에서 관측되는 정보(samples or data)들에 대한 확률분포를 추정하는 문제입니다. 그리고 이러한 density estimation을 하기 위한 수많은 기법들이 존재하지만, machine learning에서 대표적이면서 일반적인 방법으로 maximum likelihood estimatino을 가장 많이 사용하고 있습니다. Maximum likelihood estimation은 확률분포와 이 분포에 대한 parameter들이 주어졌을 때, 먼저 관측되는 정보들의 조건부 확률을 나타내는 likelihood function을 정의하고, 이에 대한 parameter들을 변화시켜 탐색(search) 또는 최적화(optimizatoin)과정을 수행함으로서 machine learning 분야에서 관측되는 정보를 나타내는 모델을 예측할 수 있는 기법으로 많이 사용되고 있습니다. 

모델을 구축하는 데에 있어서 가장 일반적인 분제는 주어진 정보에 대해서 어떻게 joint probability distribution을 추정하는지 입니다. 예를 들어서 어떠한 domain ($x_1, x_2, ..., x_n$)으로부터 관측되는 정보($X$)에 대해서 어떤 종류의 확률분포를 선택하고, 이 확률분포에 대한 parameter들을 어떻게 고정할지에 대한 문제를 의미합니다. (관측되는 정보들은 domain으로부터 각각 독립적이고, 동일한 확류분포에 의해서 추출됨을 가정으로 합니다. 이러한 개념을 independent and identically distributed(i.i.d.)라고 합니다.) 따라서, 다음과 같은 질문을 해볼 수 있습니다. 

* 어떻게 확률분포를 선택할 것인가?
* 이 확률분포에 대한 parameter들은 어떻게 고정할 것인가?

이러한 문제들을 해결할 수 있는 기법은 매우 많지만, 다음과 같이 대표적으로 두 가지가 가장 일반적입니다. 

* Maximum a Posteriori (MAP) - Bayesian method
* Maixmum Likelihood Estimation (MLE) - frequentist method

그리고 위의 문제들은 어떠한 대상으로부터 정보를 관착흘 때, 그 정보들의 양이 작고 noise가 존재할 경우에 더욱 더 어려워질 수도 있습니다. 예를 들어서 실제로 물리적인 대상으로부터 정보를 얻을 때, 일반적으로 나타나는 현상이라고 할 수 있으며, 우리가 추정한 probability density function과 그에 대한 paramter들의 결과도 마찬가지로 어느정도의 오차가 존재할 수 있다는 것을 의미합니다.  

## 3. Maximum Likelihood Estimation

Mximum likelhood Estimation (MLE)은 관측된 정보($X$)에 대한 joint probability에 대해서 최고의 결과가 나오도록 하는 parameter들을 찾는 탐색 또는 최적화 문제에 대한 해결방안이라고 할 수 있습니다. 이 과정을 설명하기 위해서 앞서 말한 parameter들은 probability density function을 선택하고, 이 분포에 대한 parameter들을 통칭하는 것으로 $\theta$로 명명하도록 하겠습니다. 이 $\theta$는 부드럽게 변화하는 숫자로서 vector로 표현이 되는 것이 보통이며, 각각의 다양한 확률분포와 그 분포에 대한 parameter들을 대표하는 것이라고 생각하면 됩니다. 그리고 maximum이라는 명칭이 붙어 있듯이, 우리는 $\theta$가 주어졌을 때 joint probabilty distribution으로부터 관측되는 정보들에 대한 확률이 최대가 되도록하는 것이 목표이며, 다음과 같이 문제를 정의할 수 있습니다. 

$$P(X|\theta)$$

이 조건부 확률은 '|' 대신해 ';'으로도 표현이 되며, 이는 $\theta$가 random variable이 아니라 모르는 parameter이기 때문입니다. 즉, 다음과 같이 표현할 수 있습니다.

$$P(X;\theta)$$ 

또는

$$P(x_1, x_2, ..., x_n; \theta)$$

그리고 이 조건부 확률로 나타나는 결과는 $\theta$에 대한 모델로부터 관측되는 정보들의 likelihood로 볼 수 있으며, 다음과 같이 $L()$로 명명합니다. 

$$ L(X;\theta)$$

따라서, Maximum likelihood estimation의 목적은 위의 likelihood function을 최대로 하는 $\theta$를 찾는 것입니다. 즉, 

$$max L(X;\theta)$$ 

라고 나타냅니다. 그리고 마찬가지로 관측되는 정보인 $X$는 각각의 관측되는 정보인 $x_1, x_2, ..., x_n$을 의미하고, 각각은 joint probability distribution을 따르기 때문에 다음과 같이 조건부확률의 곱셈식으로 표현할 수 있습니다. 

$$L(X; \theta) = L(x_1, x_2, ... , x_n;\theta)$$
$$ = \prod_{i=1}^{n} P(x_i;\theta)$$

하지만, 확률이 매우 작은 경우에는 곱셈이 이루어질 경우에 매우 불안해질 수 있으므로, 대부분의 경우 $log$ 함수를 확률에 사용하여 다음과 같이 나타냅니다. 

$$ \sum_{i=1}^{n} logP(x_i;\theta)$$

이 때, $log$는 e를 base로 하며, log-likelihood function라고 부릅니다. 그리고 대부분의 최적화 문제에서는 cost function을 최소화하는 것을 다루기 때문에 마찬가지로 maximum likelihood estimation에서도 주어진 likelihood function에 -1을 곱한 값을 최대화하는 것으로 합니다. 따라서 다음과 같이 nagative log-likelihood (NLL) function이 만들어집니다. 

$$min -\sum_{i=1}^{n} logP(x_i;\theta)$$


## 4. MLE in machine learning

앞서 설명한 density estimation은 applied machine learning과 관련이 있습니다. 그리고 machine learning model을 예측하는 것을 probability density estimation의 문제로 볼 수 있습니다. 이를 구체적으로 이야기 하자면, 모델과 모델에 대한 parameter들을 선택하는 문제를 hypothesis ($h$)를 예측하는 것이고, 이 hypothesis는 관측되는 정보를 가장 잘 설명할 수 있는 모델이라고 하며 다음과 같이 정의합니다.

$$P(X;h)$$

그러므로 이를 다시 maximum likelihood estimation 문제로서 해결하고자 하면 다음과 같이 정의할 수 있습니다.

$$max  L(X;h) = max \sum_{i=1}^{n}logP(x_i;h)$$

그리고 maximum likelihood estimation은 supervised machine learning에도 유용하게 사용됩니다. 예를 들어, 주어진 input에 대해서 output을 어떻게 mapping하느냐에 따라서 regression 또는 classificaion 문제를 해결할 수 있는 기법으로 사용되며, 다음과 같이 문제를 정의할 수 있습니다. 

$$max L(y|X;h) = max \sum_{i=1}^{n}logP(y_i|x_i;h)$$




### 4-1. MLE를 이용한 선형회귀

앞으로 머신러닝을 배우실 여러 분에게 있어서 **선형회기(learning regression)**는 가장 기본적인 이론이라고 할 수 있으며, 본 과정에서는 간략하게 설명만 하고 넘어가도로 하겠습니다. ***선형회기란 어떠한 이산 데이터가 존재할 때 이 데이터를 표현하는 분포도를 선형으로 하여금 표현하는 것을 의미합니다.** 그리고 데이터에 존재하지 않는 수치에 대해서도 예측을 할 수 있도록 하는 것이 목적이며, 선횡회기가 잘 될수록 예측 또한 오차가 적어집니다. 즉, 어떠한 모델이라는 함수 $f$가 존재할 때, 이에 대한 예측값인 $\hat{y}$는 다음과 같이 표현을 합니다.
$$ \hat{y} = f(x)$$
($\hat{}$이라는 표기인 'hat'은 보통 예측값에 많이 사용됩니다.)

그리고 이 $f$는 용어에서 알 수 있듯이, 선형으로 이루어져 다음과 같이 표현될 수 있습니다. 

$$\hat{y} = \beta_0 \times x_0 + \beta_1\times x_1 + \beta_2\times x_2 ... + \beta_n\times x_n $$
$$ = \sum^{n}_{i=0} \beta_i\times x_i $$


```python
import numpy as np
import matplotlib.pylab as plt

a = 1
b = 5
x = np.arange(0, 20, step=0.5)
y = a * x + b + np.random.normal(0, 2, size=len(x))

# plot을 위한 부분
plt.scatter(x, y, label='data')
plt.plot(x, a*x + b, c='r', label='target')
plt.legend()
plt.show()
```


![png](output_28_0.png)


위와 같이 빨간선이 우리가 찾고자하는 모델이고, 파란색 점들이 데이터입니다. 

즉, 선형회기의 목적은 위의 $\beta$라는 parameter들을 구하는 것입니다. 그리고 MLE를 적용하면, 우리가 관심있는 모델인 $f$에 대한 parametmer들인 $\beta$의 posterior 분포를 구하는 것이라고 할 수 있습니다. 우리는 이러한 모델을 만드는 과정을 probability density esitimation의 문제로서 다시 정의할 수 있습니다. 즉히, 모델과 그 모델에서 사용되는 파라미터들을 선택하는 일련의 과정을 모델링을 하는 것으로 이 모델을 $h$라는 가정을 하고, 주어진 데이터를 가장 잘 표현하는 $h$를 찾는 것이라고 할 수 있스니다. 그러므로 다음과 같이 우리는 likelihood함수를 최대화하는 $h$를 찾아야합니다.

$$ max \sum_{i=1}^n logP(x_i; h) $$

그리고 Supervised learning이 다음과 같이 conditional probability 문제로서 주어진 input에 대한 output의 확률을 예측하는 것으로 다음과 같이 표현될 수 있습니다. 

$$P(y|X)$$ 

그렇기 때문에 우리는 위의 문제를 supervised learning의 영역에서 표현을 하면 다음과 같습니다. 

$$max \sum_{1}^n logP(y_i | x_i;h) $$

이제 우리는 $h$라는 모델 또는 함수를 선형회귀 모델로 바꿀 것입니다. 이에 대해서 우리는 하나의 가정을 사용할 것인데, 이는 매우 합리적이라고 할 수 있습니다. 즉, 주어진 데이터의 측정치는 모두 독립적이고, 동일한 확률분포에 의해서 나타나는 확률변수라는 점입니다. 이는 [identical independent distribution](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)라는 정의로서 자세한 설명은 [위키](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)를 참조하시기 바랍니다. 그리고 우리가 예측하고자하는 값인 $(y)$는 평균이 0인 정규분포의 noise를 갖는다고 할 것입니다. 이러한 가정과 함께 우리는 $X$가 주어질 때 $y$를 예측하는 문제를 동일하게 $X$가 주어질 때 정규분포로부터 $y$를 얻기위한 정규분포의 평균을 예측하는 문제로 바꿀 수 있습니다. 이에 대한 수식적인 표현은 다음과 같습니다. 

$$ f(x) = \frac{1}{\sqrt{2\times \pi \times \sigma^2}} \times exp(-\frac{1}{2 \times \sigma^2}\times(y - \mu)^2)$$

여기서 $\mu$가 우리가 예측하고자하는 정규분포의 평균이고, $\sigma$는 표준편차입니다. 그리고 우리는 이 함수를 likelihood function으로 사용할 수 있습니다. 

앞선 문제의 경우에서 우리는 $h$를 표현하는 데 사용되는 $a$, $b$라는 파라미터를 구하는 것이 목적입니다. ($a$는 직선의 기울기에, $b$는 $y$절편입니다.) 그러므로 위의 식은 아래와 같이 다시 나타낼 수 있습니다. 

$$ p(y|x; h) = \frac{1}{\sqrt{2 \times \sigma^2 \times \pi}}e^{-\frac{(y - (a\times x + b))^2}{2\sigma^2}} $$

$$ log p(y|x, \theta) = -\frac{(y - (ax + b))^2}{2\sigma^2} + C $$

그리고 $log p(y|x, \theta)$를 최소화하는 $a$와 $b$를 찾는 것이 우리의 목표입니다. 이 때, 다른 항들은 $a$와 $b$의 변화에 영향을 받지 않으므로 결과적으로 $-(y - (ax + b))^2$에 대한 최소화를 하는 것과 동일하다고 볼 수 있습니다. 그리고 이에 대해서 우리는 최소자승법(least squares)이라고 부르는 방법을 많이 사용합니다. 이렇게 함수의 최소값을 찾는 것은 gradient descent라는 방식으로 하여금 조금씩 기울기를 따라 내려가면서 최소값을 찾습니다. 기울기를 따라가는 것이기 때문에 미분이 사용되며, 특정 변수에 대한 미분이므로 편비분을 하게 됩니다. 따라서, 위의 문제에 대해서는 다음과 같이 편미분이 이루어집니다.

$$ \frac{\partial (logP(y|x;h))}{\partial a} = -2(y - (ax + b))x $$
$$ \frac{\partial (logP(y|x;h))}{\partial b} = -2(y - (ax + b)) $$

이렇게 임의의 점(또는 초기의 시작점이 주어질 수도 있습니다.)에서 시작하여 gradient의 반대 방향(-가 앞에 존재하기 때문)으로 조금씩 이동하는 방식을 **stochastic gradient descent (SGD)**라 합니다.

이를 활용하여 다음과 같이 python으로 구해보도록 하겠습니다.


```python
def grad(x, y, a_hat, b_hat):
    return (-(y - (a_hat * x + b_hat)) * x, -(y - (a_hat * x + b_hat)))
```


```python
# gradient descent 계산
a_hat = 0.4; b_hat = 1; stepsize = 0.0001

theta_list = [(a_hat, b_hat)]

for epoch in range(10000):
    for feature, label in zip(x, y):
        a_grad, b_grad = grad(feature, label, a_hat, b_hat)
        a_hat -= stepsize * a_grad
        b_hat -= stepsize * b_grad
        theta_list.append((a_hat, b_hat))

print("********* Results ***********")        
print('a_hat: ', a_hat)
print('b_hat: ', b_hat)
print("********* ******* ***********")        

# plot을 위한 부분
plt.scatter(x, y, label='data')
# plt.plot(x, a*x + b, label='target')
plt.plot(x, a_hat*x + b_hat, c='r', label='regression')
plt.legend()
plt.show()

```

    ********* Results ***********
    a_hat:  1.0110228329088864
    b_hat:  4.498430138396311
    ********* ******* ***********



![png](output_32_1.png)


위의 결과에서 알 수 있듯이, 우리는 $a$는 1이고, $b$는 5인 target으로부터 데이터를 생성하여 선형모델을 구하는 것을 진행하였습니다. 그리고 그 target이 되는 parameter인 $a, b$에 대해서 각각의 예측값인 $\hat{a}, \hat{b}$는 매우 유사하게 예측이 되었음을 확인할 수 있습니다.


```python

```
