---
layout: post
title: AutoBalance
category: Deep Learning
tag: [autobalance, imbalanced data]
---


# [AutoBalance: Optimized Loss Functions for Imbalanced Data](https://arxiv.org/abs/2201.01212)


### Abs.

* a bi-level optimization framework that automatically designs a training loss function to optimize a blend
of accuracy and fairness-seeking objectives. 

* Specifically, a lower-level problem trains the model weights, and an upper-level problem tunes the loss function by monitoring and optimizing the desired objective over the validation data. 

* Our loss design enables personalized treatment for classes/groups by employing a parametric cross-entropy loss and individualized data augmentation schemes. 




### Intro.

* Besides class imbalance, minorities can also appear at the feature-level; for instance, the specific values of the features of an example can vary depending on that example’s membership in certain sensitive or protected groups, e.g. race, gender, disabilities.


* **How to design a training loss to maximize a fairness-seeking objective on the test set?**

    * Bayes-consistent loss functions: weighted cross-entropy

    > Unfortunately, this intuition starts to break down when the training problem is overparameterized, which is a common practice in deep learning: `overparameterized`란 모델의 파라미터의 수가 데이터의 수보다 많아서, loss를 최소화하면서 학습을 하는 것이 아니라 단순히 데이터를 그대로 외우는 `overfitting`도 일어날 수 있다는 의미.

    * In fact, recent works [8, 42] show that weighted cross-entropy has minimal benefit to balanced accuracy
        
        * alternative methods based on margin adjustment can be effective (namely, by ensuring that minority classes are further away from decision boundary).

        * These ideas lead to the development of a parametric cross-entropy function:
            $$
            \ell(y, f(x)) = w_y \log \left( 1 + \sum_{k \neq y} e^{l_k - l_y} \cdot e^{\Delta_k f_k(x) - \Delta_y f_y(x)} \right)
            $$

            , whilch allows for a personalized ptreatment of the individual classes via the design parameters $(w_k, l_k, \Delta_k)_{k=1}^K$

            $w_k$ is the classical weighting term whereas $l_k$ and $\Delta_K$ are additive and multiplicative logit adjustments.

        *  ***it is unclear how such parametric cross-entropy functions can be tuned for use for different fairness objectives, for example to tackle class or group imbalances.***



    * main idea is to use bi-level optimization, where the model weights are optimized over the training data, and the loss function is automatically tuned by monitoring the validation loss. Our core intuition is that unlike training data, the validation data is difficult to fit and will provide a consistent estimator of the test objective.

        * novel strategies that narrow down the search space to improve convergence and avoid overfitting

        * so incorporates data augmentation policies personalized to subpopulations (classes or groups)



## References 

- [AutoBalance: Optimized Loss Functions for Imbalanced Data](https://arxiv.org/abs/2201.01212)
