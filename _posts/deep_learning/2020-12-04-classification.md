---
layout: post
title: Classification
category: Deep Learning
tag: classification
---

# Classification (120분)

**Classification**이란 말그대로 범주화/분류화를 하는 것을 의미합니다. 데이터에 빗대어서 설명하자면, 여러가지 내용을 포함하는 데이터가 있을 때, 이 데이터를 해석하여 이해하기 쉽도록 정리하는 것이라고도 할 수 있습니다. 예를 들어, 실생활에서 많이 사용되는 이메일에 스팸을 자동으로 필터링해주는 기능이 있습니다. 과거에는 스팸을 신고하고 신고한 메일주소에서 수신되는 메일에 대해서만 스팸으로 범주화하였지만, 최근에는 머신러닝을 통하여 메일의 제목 또는 발송 주소 등의 데이터를 해석하여 스팸을 자동으로 필터링해주고 있습니다. 즉, 주어진 데이터가 있는데 이 데이터는 어떤 범주에 속하는지를 판단해주는 기능을 합니다. 이렇듯, classification은 실생활에서 매우 근접하게 사용되고 있는 머신러닝의 영역 중 하나로서 주어진 데이터의 판별을 위한 방법으로 많이 사용되고 있습니다. 

본 강의는 머신러닝에서의 classification에 대해서 배울 것이며, 그 기초가 되는 **Naive Classifier**에 대해서 먼저 알아보도록 하겠습니다. Naive classifer는 주어진 문제에 대해서 어떤 가정도 하지 않은 모델로서 성능을 비교하고자 하는 다른 모델들의 기준이 되는 역할을 하는 classifier를 의미합니다. 이러한 Naive classifer를 설계하는 데에는 주어지는 데이터셋과 성능지표에 따라 여러 가지 방법이 존해합니다. 그리고 가장 보편적으로 사용되는 성능 지표로는 주어진 데이터로부터 임의로 선택된 class의 label을 추측하는 데에 있어서 얼마나 정확하게 선택한 class에 대해서 label을 많이 맞추는지의 분류 정확도(classification accuracy)를 사용합니다. 


## 1. Naive classifier

Classification을 통한 주어진 데이터를 판단하는 문제는 데이터로부터의 어떤 class를 모델에 input으로 넣었을 때 그에 맞는 label을 얻는 것이고, classificaion을 위한 모델은 학습용 데이터에 대해서 학습을 진행하고 테스트용 데이터를 통해서 검증을 함으로서 성능을 결정합니다. 그리고 이에 대한 성능인 정확도(accuracy)는 예측을 하여 맞은 횟수를 예측을 한 총 횟수로 나눈 것으로 나타냅니다. 하지만, 이 정확도는 어떤 classificaion을 위한 모델에 대한 성능을 수치로서 나타낼 뿐이며, '이 모델이 성능이 좋은가? 좋지 못한가?'에 대해서는 답이 되지 못합니다. 그렇기 때문에 비교의 기준이 되는 모델이 필요한 것이고, naive classifier가 이 기준이 되는 것입니다. 

***naive classifier는 기본적으로 예측을 하기 위해서 어떠한 기법도 사용하지 않는 무작위한 또는 동일한 예측을 하도록 합니다.*** 그렇기 때문에 말 그대로 naive라는 표현을 하는 것이고, 다시 말해서 domain에 대해서 어떠한 사전지식을 사용하지 않으며 예측을 하기 위해서 어떠한 learning도 없습니다. 그리고 naive classifier는 앞서 말했듯이 기준점이 되는 것이므로 설계한 classification model에 대해서 최소 성능(lower bound)이 된다고도 할 수 있습니다. 즉, classification model을 설계하고자 한다면 최소한 naive classifier보다는 성능이 좋게 나와야합니다. 예를 들어서 naive classifier보다 성능이 좋지 못하다면, 설계한 classification model은 분류라는 성능이 없다고 보아도 됩니다. 그렇다면 navie classifier는 어떻게 설계를 해야하는지에 대해서 알아보겠습니다. 

* random class 예측


* Randomly selected class 예측 


* Majoriy class 예측

위와 같이 여러 naive classifier가 존재하며 성능이 동일하지 않을 것입니다. 그리고 우리는 앞으로 설계할 classifier의 성능을 최대한 좋게 만드는 것이 목적이기 때문에 비교대상이 되는 naive classifier도 마찬가지로 주어진 문제에서 가장 성능이 좋은 것을 사용해야합니다.

먼저, 앞으로의 naive classifier를 설명하기 위한 데이터셋을 다음과 같이 하도록 하겠습니다. 


```python
# 테스트 데이터셋
class_0 = [0 for _ in range(20)]
class_1 = [1 for _ in range(80)]
y = class_0 + class_1

# 각 분포에 대한 프린트
print( ' Class 0: ', (len(class_0) / len(y) * 100))
print( ' Class 1: ', (len(class_1) / len(y) * 100))
```

우리는 두 가지로 구분이 되는 데이터셋(`class 0`, `class 1`)을 설정하였습니다. 그리고 각각의 `class`에 대해서 데이터셋의 비율은 1:3으로 분포가 이루어져 있도록 설정하였습니다. 즉, 전체 데이터의 `class` 중에서 `class 0`이 20%이고, `class 1`이 80%의 비율로 구성되어 있다는 것을 확인할 수 있습니다. 

그리고 우리는 naive classfier의 성능을 검증할 수 있는 확률을 다음과 같이 정의할 수 있습니다.

$$ P(\hat{y} = y)$$

이는 다음과 같이 각각의 경우에 대한 확률을 통해서 계산됩니다. 

$$ P(\hat{y} = y) = P(\hat{y} = 0) \times P(y = 0) + P(\hat{y} = y) \times P(y = 1) $$

이를 통해서 우리는 주어진 데이터에 따르는 모델의 성능을 확인할 수 있습니다. 특히, 이렇게 확률을 활용하면 계산이 매우 간단하다는 점에서 우리는 대부분의 다른 방식의 naive classifier에도 통용이 가능하다는 점에서 이점을 가지고 있습니다. 

### 1-1. Random Guess 예측

**random-guess strategy**은 가능한 class에 대해서 예측을 할 때, 무작위로 답을 고르는 것입니다. 각각의 class에 대하여 무작위로 추측하는 것은 가능한 각 class의 label에 대해서 uniform probability distribution을 통해 이루어지고, 앞선 예제와 같이 두 가지의 class를 가지는 경우에는 각각의 class에 대해서 0.5의 확률을 갖게 됩니다. 또한, 앞서 설명하였듯이 우리는 `class 0`과 `class 1`에 대해서 expected probability가 각각 0.25, 0.75인 것을 알고 있습니다. 그러므로 우리는 이 방식에 따른 평균 성능을 다음과 같이 계산할 수 있습니다. 

$$P(\hat{y} = y) = P(\hat{y} = 0) \times P(y = 0) + P(\hat{y} = 1) \times P(y = 1)$$
$$ = 0.5 \times 0.2 + 0.5 \times 0.8$$
$$ = 0.5$$

따라서, 주어진 문제에 대해서 uniform probability distribution에 의해서 무작위로 class의 label을 추측하는 것의 성능은 50%의 classification accuracy를 갖는다는 것을 확인할 수 있습니다. 

다음은 0.5의 확률로 random guess를 하는 파이썬 코드입니다.

```python
def randomly_predict():
    if random() < 0.5:
        return 0
    return 1
```

위와 같이 하나의 모듈로서 작성한 것은 iteration을 통해 연속적으로 random guess를 해야하는 경우에 편의성을 제공하기 위함입니다. 여러분들도 앞으로는 하나의 기능을 수행하는 함수에 대해서는 모듈로서 작성하여 불러서 사용하는 것을 권장합니다. 즉, 다음과 같이 1000번의 예측을 한느 경우에 대해서는 위와 같이 function을 작성하여 각각의 예측마다 불러서 활용할 수 있도록 call-back function으로 적용합니다. 


```python
import numpy as np
from sklearn.metrics import accuracy_score
```

먼저 계산에서 사용할 수식이 들어 있는 라이브러리를 불러옵니다.



```python
def randomly_guess():
    if np.random.random() < 0.5:
        return 0
    return 1
```

무작위한 수를 생성했을 때, 0.5보다 작으면 0을 return하고, 0.5보다 큰 경우에는 1을 return하는 함수입니다. 함수명에서도 알 수 있듯이 무작위하게 0과 1을 return해주는 것을 알 있습니다.


```python
class_0 = [0 for _ in range(20)]
class_1 = [1 for _ in range(80)]
y = class_0 + class_1
```


```python
# 성능값 계산
res = []
for _ in range(1000):
    y_hat = []
    for _ in range(len(y)):
        y_hat.append(randomly_guess())
    acc = accuracy_score(y, y_hat)
    res.append(acc)
    
print('평균 정확도: ', np.mean(res))
```

따라서, 1000번의 예측을 했을 때 모델의 성능은 기존에 우리가 확률을 이용하여 예상한 값인 0.5와 매우 비슷하다는 것을 확인할 수 있습니다. 이는 1000번이라는 실험 횟수가 적당했다는 것을 의미하고, 10번 또는 횟수가 적어질수록 0.5에서 멀어지는 값이 나올 것입니다. 즉, 주어진 데이터를 통해서 모델의 정확도의 성능은 우리가 확률을 통해서 계산하는 값으로 수렴하기 위해서는 그만큼 충분한 실험 횟수가 필요하다는 것을 알 수 있습니다. 9가 나왔습니다. 이 에제의 경우에는 기존의 한 번의 예측에 대한 정확도가 1000번의 예측에 정확도와 매우 비슷하다는 것을 확인할 수 있습니다. 이는 [Law of large numbers](https://ko.wikipedia.org/wiki/%ED%81%B0_%EC%88%98%EC%9D%98_%EB%B2%95%EC%B9%99)라는 법칙으로서 참조하시기 바랍니다. 

> Low of large numbers란 위의 참조에서 더욱 자세히 알 수 있지만, 간단히 설명하자면 다음과 같습니다. 어떠한 큰 모집단에서 무작위로 뽑은 표본의 평균이 전체 모집단의 평균과 가까울 가능성이 높다는 통계와 확률 분야의 기본 개념이다.

### 1-2. Randomly Selected Class 예측

이번에는 다른 방식으로 naive classifier를 설계해보도록 하겠습니다. 우리는 학습용 데이터셋에서 관측 데이터를 무작위로 선택할 수 있고 각각의 예측에 대해서 그 값을 결과로 제시할 수 있습니다. 그리고 아마도 학습용 데이터셋을 활용하는 방식이 오히려 **1-1**처럼 무작위적으로 추측을 하는 방식보다는 좀 더 좋은 결과를 보여줄 수 있을 것입니다. 

이번에도 마찬가지로 확률을 활용하여서 접근하고자 하는 방식에서 기대되는 성능을 계산할 수 있습니다. 만약 uniform probability distribution으로 학습용 데이터셋으로부터 관측되는 데이터 샘플을 선택한다면, `class 0`의 확률은 20%이고, `class 1`의 확률은 80%가 될 것입니다. 그리고 이는 각각 독립적입니다. 이러한 것을 고려한다면, 다음과 같이 계산식을 세울 수 있습니다. 

$$P(\hat{y} = y) = P(\hat{y} = 0) \times P(y = 0) + P(\hat{y} = 1) \times P(y = 1)$$
$$ = 0.2 \times 0.2 + 0.8 \times 0.8$$
$$ = 0.68$$

즉, 약 68%의 정확도를 예상할 수 있다는 것이고, 이는 앞선 무작위로 추측하는 방식(50%)보다 더 좋은 결과라는 것을 확인할 수 있습니다. 

```python
def randomly_predict(y):
    selected_index = np.random.randint(len(y))
    
    return y[selected_index]
```

이렇게 function으로 만들었기 때문에 1000번의 예측을 하는 반복문에서 활용하기 용이합니다. 그리고 앞선 예제와 마찬가지로 평균 성능을 계산하여 기존에 우리가 예측한 정확도가 나오는지 확인하겠습니다. 


```python
import numpy as np
from sklearn.metrics import accuracy_score
```

먼저 계산에서 사용할 수식이 들어 있는 라이브러리를 불러옵니다.



```python
# randomly selected class로부터 예측값을 얻어내는 func.
def randomly_select(y):
    selected_index = np.random.randint(len(y))
    
    return y[selected_index]
```


```python
# 데이터셋
class_0 = [0 for _ in range(20)]
class_1 = [1 for _ in range(80)]
y = class_0 + class_1
```

0과 1에 대한 데이터 셋을 1:3의 비율로 만듭니다.


```python
# 성능값 계산
res = []
for _ in range(1000):
    y_hat = []
    for _ in range(len(y)):
        y_hat.append(randomly_select(y))
    acc = accuracy_score(y, y_hat)
    res.append(acc)
    
print('평균 정확도: ', np.mean(res))
```

앞서, 우리는 확률을 이용한 계산에서 정확도가 약 68%가 나오는 것을 기대하였고, 1000번의 예측에 대한 평균 정확도는 위의 결과와 같이 비슷하다는 것을 확인할 수 있었습니다. 첫번쨰의 방법은 정말 단순하게 적용한 것이고, 두 번째에는 첫 번쨰 방법에서 학습용 데이터셋을 활용한다는 점이 개선되었습니다. 그렇다면, 앞선 둘의 공통점인 uniform probability distribution을 활용하지 않는다면 더 개선된 정확도를 볼 수 있을지 다음에서 확인해보도록 하겠습니다.

### 3. Majority Class 예측

이전에는 모두 uniform probability distribution을 사용하여 class의 label을 예측하였습니다. 하지만, 저희가 정의한 데이터셋은 각각의 두가지의 `class`가 존재하고 각각이 차지하는 비율이 1:1이 아닙니다. 즉, **1-1**과 **1-2**에서 가정하였던 uniform probability distrbution은 좋지 않은 추측이었다고 할 수 있습니다. 그렇기 때문에 이번에는 이를 개선한 naive classifier를 소개하도록 하겠습니다. 

대신에 우리는 다수의 class를 예측하여 적어도 학습용 데이터셋에서 다수의 class가 차지하는 비율만큼 높은 정확도를 얻을 수 있습니다. 예를 들어서, 학습용 데이터셋에서 `class 1`이 차지하는 비율이 80%이고, `class 1`을 모두 예측한다면, 우리는 적어도 정확도가 80%보다 높은 naive classifier를 설계할 수 있습니다. 그리고 앞선 방식들과 마찬가지로 확률을 활용하여 이를 확인하도록 하겠습니다. 주의할 점은 `class 0`의 확률을 0으로 할 것이고, `class 1`의 확률은 1로 하도록 하겠습니다. 

$$P(\hat{y} = y) = P(\hat{y} = 0) \times P(y = 0) + P(\hat{y} = 1) \times P(y = 1)$$
$$ = 0.0 \times 0.2 + 1.0 \times 0.8$$
$$ = 0.8$$

위의 계산을 통해서 우리는 다수의 class를 예측하게 된다면 80%라는 높은 정확도를 가질 수 있음을 확인하였습니다. 이는 앞선 두 방식보다도 높은 성능을 나타내지만, 데이터에서 각각의 `class`가 차지하는 비율이 한쪽으로 치우치는 경우가 심할 수록 높은 정확도를 가진다고 할 수 있습니다. 그렇기 때문에 데이터가 구성되는 비율이 어떻게 되어 있는지에 대해서 판단을 하고 사용을 해야한다는 것을 다시 한번 확인할 수 있습니다. 

```python
def select_majority(y):
    return mode(y)[0]
```

이 때, scipy의 python library에서 mode()라는 function이 사용되는 데, 이는 주어진 argument에서 가장 보편적인 값을 찾아주는 것입니다. 따라서, 데이터를 argument로 넘기면 가장 다수를 차지하는 값을 찾아줍니다. 그리고 마찬가지로 여러 번의 예측 횟수를 통해서 평균 정확도를 계산할 것입니다. 하지만, 앞선 두 방식처럼 단순히 예측횟수를 늘리지 않을 것이며, 데이터셋의 크기만큼만 반복할 것입니다. 이는 앞선 두 방식에서는 무작위로 선택한다는 점이 있지만 이번에는 무작위로 하는 행위가 포함되어 있지 않기 때문입니다. 


```python
from scipy.stats import mode
from sklearn.metrics import accuracy_score
```


```python
def select_majority(y):
    return mode(y)[0]
```


```python
# 데이터셋
class_0 = [0 for _ in range(20)]
class_1 = [1 for _ in range(80)]
y = class_0 + class_1
```

0과 1에 대한 데이터 셋을 1:3의 비율로 만듭니다.


```python
# 예측값
y_hat = []
for _ in range(len(y)):
    y_hat.append(select_majority(y))

# 정확도 계산
accuracy = accuracy_score(y, y_hat)
print('정확도: ', accuracy)
```

위의 결과를 통해서 정확도가 75%로서 우리가 예측한 값도 동일하게 나온 것을 확인할 수 있습니다. 이는 앞선 두 방식보다도 높은 것이므로, 우리는 앞서 말했듯이 여러 naive classifier를 설계할 수 있지만, 그 중에서 가장 성능이 좋은 것을 선택해야하므로 다수의 class를 활용하는 세 번째 방식을 택해야할 것입니다. 

## 2. Naive Bayes Classifier


앞서 배운 naive classifier는 사실 데이터 자체를 그대로 사용하고 있으며, 데이터에서 얻을 수 있는 다른 정보들은 사용하지 않았습니다. 아마도 그렇기 때문에 다른 고차원적인 classifier의 성능을 판별할 수 있는 기준이 되는 것이기도 하다고 생각합니다. 이번에는 데이터에서 얻을 수 있는 정보를 활용하는 **naive bayes classifier**에 대해서 알아보도록 하겠습니다. naive bayes classifier는 이름에서도 알 수 있듯이, bayes theorem의 심플하면서도 강력한 계산식을 활용하는 classifier라고 할 수 있습니다. 그리고 bayes theorem에서 가장 중요한 부분이 condiditional probability를 다루어서 계산식을 정리한다는 점이고, 마찬가지로 classification 문제에서도 주어진 데이터에 대해서 conditional probability를 정의함으로서 classificaion을 수행합니다. 

설명하기에 앞서, 앞선 설명에서처럼 수식을 다루는데에 있어서 observation이나 input에 해당하는 데이터는 $X$라고 지칭하고, 모델을 통해서 생성되는 또는 예측되는 결과값을 $y$라고 명명하도록 하겠습니다. 따라서, 모델을 $X$에 대한 $y$로의 mapping이라고 생각하면 함수 $f$라고 생각할 수 있기 때문에 일반적으로 다음과 같이 보편적으로 간단하게 표현하기도 합니다. 

$$ y = f (X) $$

그리고 $f$를 만들기 위한 또는 학습시기키 위한 지표가 있어야 하며, 이 지표는 cost 또는 loss라고 말하며 classification에 대한 error를 낮추는 것을 목표로 합니다. 

앞서 말했듯이, bayes theorem을 활용하는 classifier이기 때문에 데이터를 확률적인 측면으로 재해석할 필요가 있습니다. 그리고 이때 conditional probability가 고려되는 것이며, 이 conditional probability를 계산하기 위한 방법이 naive bayes인 것입니다. 따라서, 우리가 데이터로부터 알고자 하는 것은 데이터가 주어질 때, 이 데이터가 어느 범주에 속하느냐는 conditional probability라는 것입니다. 예를 들어서, 다음과 같이 $y_1 , y_2 , · · · , y_k$ 에 대해서 $X_1 , X_2 , · · · , X_n, X = {x_1, x_2, ,,, x_n}$의 데이터가 주어졌을 때, 우리는 다음과 같이 conditional probability를 정의할 수 있습니다. 

$$P (y_i |x_1 , x_2 , · · · , x_n )$$

이에 대해서 bayes theorem은 다음과 같습니다. 

$$ P(A|B) = \frac{P(B|A) \times P(A)}{P(B)} $$

그리고 이러한 이론을 앞선 classification 문제에 적용하면 다음과 같이 계산이 가능하도록 우리가 원하는 결과를 얻을 수 있는 것입니다. 

$$P(y_i|x_1, x_2, ..., x_n) = \frac{P(x_1, x_2, ..., x_n|y_i)\times P(y_i)}{P(x_1, x_2, ..., x_n)} $$

먼저, 다음과 같이 $P(y_i)$는 주어진 데이터셋으로부터 쉽게 얻을 수 있는 확률입니다. 

$$ P(y_i) = \frac{y_i에 해당하는 데이터 샘플}{전체 데이터 샘플} $$

그리고 우리는 각각의 데이터는 서로 독립적으로 서로에게 영향을 미치지 않는다고 가정을 하여 다음과 같은 이득을 얻을 수 있습니다. 

* 위의 계산식에서 분모에 해당하는 부분는 상수로 취급을 할 수 있게 되기 때문에 단순한 nomalization으로 생각하면 됩니다.


* conditional probability는 다음과 같이 계산이 가능하도록 됩니다. 

$$ P (y_i |x_1 , x_2 , · · · , x_n ) = P (x_1 , x_2 , · · · , x_n |y_i) × P (y_i)$$ 

$$ P (y_i |x_1 , x_2 , · · · , x_n ) = P (x_1|y_i)\times P(x_2|y_i)\times ...\times P(x_n|y_i)\times P(y_i)$$ 


        


```python
from sklearn.datasets import make_blobs
import matplotlib.pyplot as plt
import numpy as np
from scipy.stats import norm
```


```python
X, y = make_blobs(n_samples=100, centers=2, n_features=2, random_state=1)

print('input 데이터: ', X.shape)
print('output 데이터: ', y.shape)

plt.scatter(X[:, 0], X[:, 1], marker='o', c=y, s=100,
            edgecolor="k", linewidth=2)
plt.xlabel("$X_1$")
plt.ylabel("$X_2$")
plt.show()
```

    input 데이터:  (100, 2)
    output 데이터:  (100,)



![png](output_37_1.png)


위에서 알 수 있듯이, `make_blobs`를 활용하여 2개의 `class`로 구분이 가능한 데이터 셋을 생성하였습니다.

이번에는 앞서 말했듯이, 주어진 데이터 셋에 대한 확률을 정의하는 모듈을 만들어 보도록 하겠습니다. 이 때, 우리는 데이터에 대한 확률을 정의하는 데에 있어서 정규분포를 이용할 것이며, 이는 python에서 Scipy에서 제공하는 정규분포 함수를 활용하여 다음과 같이 데이터의 평균과 분산을 통해서 구현이 가능합니다. 


```python
def make_distribution(data):
    mu = np.mean(data)
    sigma = np.std(data)
    print(mu, sigma)
    
    distr = norm(mu, sigma)
    
    return distr
```

이제는 각각의 `class`에 대해 해당하는 conditional probabiliyt를 정의하도록 하겠습니다. 이를 위해서 앞서 만든 데이터셋을 각각의 `class`로 구분해야합니다. 


```python
X_0 = X[y == 0]
X_1 = X[y == 1]

print('class 0에 대한 input: ', X_0.shape)
print('class 1에 대한 input: ', X_1.shape)
```

    class 0에 대한 input:  (50, 2)
    class 1에 대한 input:  (50, 2)


그리고 prior probability는 다음과 같이 쉽게 구할 수 있습니다. 


```python
p_y_0 = len(X_0) / len(X)
p_y_1 = len(X_1) / len(X)

print('P(y_0) = {}'.format(p_y_0))
print('P(y_1) = {}'.format(p_y_1))
```

    P(y_0) = 0.5
    P(y_1) = 0.5


위에서 정의한 계산식에 맞추어 각각의 데이터를 분리하여 다음과 같이 분포로서 확률을 정의합니다. 


```python
dist_X_1_y_0 = make_distribution(X_0[:, 0])
dist_X_2_y_0 = make_distribution(X_0[:, 1])
dist_X_1_y_1 = make_distribution(X_1[:, 0])
dist_X_2_y_1 = make_distribution(X_1[:, 1])
```

    -1.5632888906409914 0.787444265443213
    4.426680361487157 0.958296071258367
    -9.681177100524485 0.8943078901048118
    -3.9713794295185845 0.9308177595208521


마지막으로, conditionaly probability를 계산하는 함수를 다음과 같이 만듭니다. 


```python
def conditional_prob(X, prior, dist_1, dist_2):
    return prior * dist_1.pdf(X[0]) * dist_2.pdf(X[1])
```

전체 데이터 중에서 10번째 데이터에 대해서는 다음과 같이 구할 수 있습니다. 


```python
X_10, y_10 = X[10], y[10]
p_y_0 = conditional_prob(X_10, p_y_0, dist_X_1_y_0, dist_X_2_y_0)
p_y_1 = conditional_prob(X_10, p_y_1, dist_X_1_y_1, dist_X_2_y_1)
print('P(y=0| {}) = {}'.format(X_10, p_y_0*100))
print('P(y=1| {}) = {}'.format(X_10, p_y_1*100))
print('Truth: y= ', y_10)
```

    P(y=0| [-10.03640801  -5.5691209 ]) = 6.471562626475902e-98
    P(y=1| [-10.03640801  -5.5691209 ]) = 0.0819979322731152
    Truth: y=  1


따라서 위의 결과로서 데이서 셋에서 10번쨰에 해당하는 $X$는 확률이 더 높은 `y=1`인 경우에 대해서 더 높게 나왔기 때문에 `Truth`값과 동일하게 `class 1`에 속한다고 할 수 있습니다. 
