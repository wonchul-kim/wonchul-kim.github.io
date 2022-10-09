---
layout: post
title: Self-supervised Learning
category: self-supervised learning
tag: self-supervised
---

***labeled data을 만드는 것은 비용이 많이 들기 때문에 unlabeled data만으로 task-agnostic하게 데이터를 잘 표현하는 representation을 얻을 수 있으면 좋지 않을까?" 얻으면 얼마나 좋을까? "unsupervised learning을 통해 좋은 representation을 얻는다면 다양한 downstream task에 빠르게 적응할 수 있을 것이다, 더 나아가서는 supervision보다 더 좋은 성능을 얻을 수 있을까?***

따라서, **Self-supervised learning**은 unlabeled data로부터 좋은 representation을 만들기 위한 학습방식으로 label(y) 없이 input(x) 내에서 target으로 쓰일만 한 것을 스스로 task를 정해서 supervision 방식으로 학습한다. 
그래서 self-supervised learning의 task를 pretext task(=일부러 어떤 구실을 만들어서 푸는 문제)라고 부르며, pretext task를 학습한 모델은 downstream task에 transfer하여 사용할 수 있다. 
self-supervised learning의 목적은 downstream task를 잘푸는 것이기 때문에 기존의 unsupervised learning과 다르게 downsream task의 성능으로 모델을 평가한다.



Self-supervised learning은 큰 카테고리로 보면 Self-prediction과 Contrastive learning으로 나눌 수 있다.

#### self-prediction
