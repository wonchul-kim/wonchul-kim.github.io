---
layout: post
title: Cross Entropy Loss
category: Deep Learning
tag: [losses]
---


## Cross Entropy Loss

$$ H(p, q) = - \sum_ip(x_i)log(q(x_i))$$

    * $p(x_i)$: 실제 정답 분포 (one-hot encoding이므로 정답 클래스는 1, 나머지는 0)
    * $q(x_i)$: 모델이 예측한 클래스의 확률 분포

$$ -log(\frac{e^{z_{target}}}{\sum_je^{z_j}}) = log(\sum_{j}e^{z_j}) - z_{target}$$

    * $p(x_i)$: one-hot encoding이므로 1외에는 0이므로 무시됨
    * $z_target$: 정답 클래스에 해당하는 logit
    * $z_j$: $j$ 클래스에 대한 logit


#### exponential을 사용하는 이유:

    * 모델로부터 나온 output의 각 logit의 상대적인 크기를 유지하면서 모든 값을 양수로 만든다.

    * 모델의 output은 각각의 클래스를 대변하는 중요도(?)를 나타내지만, 정해진 범위가 없기 때문에 상대적으로 어떤게 더 중요한지 선택할 수 없습니다. (기준이 없음)

    * exponential func.의 특성상, 큰 값과 작은 값의 차이를 더 크게 하여, 클래스 간의 상대적 차이가 더 명확하게 드러난다.


#### softmax를 사용하는 이유: 

    * 합이 1이 되도록 함으로서 (정규화와 동일한 역할) 예측값을 토대로 확률분포를 만들고, 모델의 output(예측)이 확률처럼 해석될 수 있도록 한다.
    
    * 따라서, 딥러닝 모델의 학습은 특정 확률 분포를 따르는 output을 출력할 수 있도록 하는 과정이다. 

#### logit:

    * 주로 확률을 나타내는 값을 선형 변환하여 특정함수(일반적으로 softmax, sigmoid)를 적용하기 전의 값을 의미
        * logit 함수: $logit(p) = log(\frac{p}{1 - p})$
        * 0과 1사이의 확률을 실수 범위로 변환

    * 딥러닝에서는 보통 모델의 마지막 층에서의 출력값을 의미

#### log를 사용하는 이유:

    * 입력 값이 0에 가까울수록 출력이 매우 큰 음수 값을 가지는 특성이 있으므로, 잘못된 예측에 대해서 더 큰 패널티를 줄 수 있음.
        * 정확한 예측: 
            * 모델이 정답 클래스에 대해 높은 확률을 예측할 경우, 로그 값은 작고, 손실 값도 작음
            * 이는 모델이 제대로 예측했음을 의미하며, 이 경우 패널티가 작음
        * 부정확한 예측:
            * 모델이 정답 클래스에 대해 낮은 확률을 예측할 경우, 로그 값은 매우 큰 음수 값을 가지며, 손실 값이 커짐
            * 이는 모델이 잘못 예측했음을 의미하며, 이 경우 손실을 크게 주어 모델을 개선하도록 함



    * 곱셈 -> 덧셈으로 변환

    * 로그 함수는 확률이 작을수록 더 큰 값을 반환하기 때문에, 작은 확률 값을 더욱 강조.
        * 모델이 정답 클래스에 낮은 확률을 할당할수록 손실이 커지므로, 학습 과정에서 모델이 정답 클래스에 더 높은 확률을 할당하도록 강하게 유도
        * 이로써 잘못된 예측이 크게 패널티를 받아, 모델이 더 정확하게 학습




