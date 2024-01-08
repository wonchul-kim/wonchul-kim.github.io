---
layout: post
title: Bag of Tricks
category: Computer Vision
tag: [object detection]
---

인공지능과 관련된 많은 논문들이 정확도를 0.01%라도 개선하기 위해서 엄청난 computation time과 cost를 통해서 노력을 하고 있는 것에 반해, [An End-to-End Deep Learning Benchmark and Competition - DAWNBench](https://dawn.cs.stanford.edu/benchmark/)에서는 **training time, training cost, inference latency, inference cost**에 중점을 두고 있다.

해당 사이트에 가보면, ImageNet, CIFAR10, SQuAD의 데이터셋에 대해 진행되었고, 이 중에서 [CIFAR10에 대해서 단일 GPU로 94.1%의 정확도를 약 26초만에 얻은 전략을 정리해 놓은 글](https://myrtle.ai/learn/how-to-train-your-resnet-8-bag-of-tricks/)을 살펴보고자 하고, [colab](https://colab.research.google.com/github/davidcpage/cifar10-fast/blob/master/bag_of_tricks.ipynb)도 제공하고 있다.

Baseline으로 학습한 시간은 75초이고, 이를 26초로 줄여나가는 전략들을 소개하고 있다. 이에 대해서 기본적은 목표가 94%이상이라는 정확도와 함께 computation time과 cost를 줄이는 것이기 때문에 94.1% 정확도가 넘어가는 경우, 그 차이만큼 epoch 수를 줄여나가서 94.1%의 정확도로 계속 고정하여 computation time과 cost를 줄이고 있다.

1. Preprocessing on the GPU (~70초)
이미지의 normalizing, transposing, 패딩과 같은 전처리 뿐만 아니라 Augmentation 까지도 GPU를 활용해서 처리. (이 때, 데이터강화는 기존 CPU의 샘플 하나단위가 아닌 배치단위로 처리)

2. Moving max-pool layers (~64초)
conv -> batch norm -> act -> max_pool의 순서를 conv -> max_pool -> batch_norm -> act로 변경. (레이어의 이미지 크기를 줄인 후, 계산작업을 수행해서 시간 단축)

3. Label smoothing (~59초)
뉴럴넷을 generalization 시켜주고, 학습시간을 줄여주는것으로 알려진 Label Smoothing 기법을 추가.(94.2%의 정확도)

4. CELU activations (~52초)
Continuously Differentiable Exponential Linear Unit의 약자로, ELU와 비슷하지만 약간 더 가벼움. (94.3%의 정확도)

5. Ghost batch norm (~46초)
학습 배치를 여러개의 부분으로 쪼개서, 각 부분마다 독립적으로 batch norm을 적용하는 기법 (94.2%의 정확도)

6. Frozen batch norm scales (~43초)
학습시 레이어별 weight와 bias 값을 추적한 결과, 크게 변동사항이 없다는것을 발견. 고정된 Batch Norm 스케일을 적용함 (94.2%의 정확도)

7. Input patch whitening (~36초)
global PCA whitening 기법 대신, 패치 기반의 PCA Whitening 기법을 적용 (94.3%의 정확도)

8. Exponential moving averages (~34초)
학습률의 값을 조정해 나가기 위하여 5개의 배치마다 moving average 값을 업데이트 (94.3%의 정확도)

9. Test-time augmentation (~26초)



## references
* 원글: https://myrtle.ai/learn/how-to-train-your-resnet-8-bag-of-tricks/
* Colab: https://colab.research.google.com/github/davidcpage/cifar10-fast/blob/master/bag_of_tricks.ipynb
