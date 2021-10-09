---
layout: post
title: Anomaly detection
category: Computer vision
tag: anomaly detection
---

## Anomaly detection

Anomaly detection은 정상적인 sample들 사이에서 비정상, 이상치, 특이치 등의 비정상적인 sample들을 구별해내는 문제를 의미하며, 딥러닝에서는 이 문제를 풀기 위해서 다음과 같이 주어진 데이터셋의 조건에 따라 크게 3가지로 나눌 수 있다. 


#### Supervised 
* 정상 sample과 비정상 sample의 dataset이 모두 존재
* <U>dataset이 적절하게 있다면 높은 정확도를 얻을 수 있음</U>

* <U>하지만, 비정상 sample의 dataset을 얻는데 시간과 비용이 많이 소요됨</U>
    * 대부분의 실제 산업에서는 비정상 sample은 얻기가 힘들기 때문에 심각한 class imbalance 문제가 발생하기 마련이다. 

<br/>

#### Semi-supervised
* **one-classs classification**이라고도 하며, 정상 sample이외에는 모두 비정상 sample로 간주하는 방법
    * 정상 sample에 대한 normal distribution의 discriminative boundary를 설정하고, 이를 벗어나는 sample을 비정상으로 간주
    * 그렇기 때문에 이 boundary를 어떻게 잘 설정하느냐가 중요하며, <U>정상 sample만 있어도 가능</U>
    
    * <U>하지만, 정확도가 **Supervised**보다 떨어질 수 있음</U>
    
* 참조 논문
    * [One-Class SVM](https://www.jmlr.org/papers/volume2/manevitz01a/manevitz01a.pdf)을 대표로 참조 가능
    * 딥러닝을 활용한 방안으로는 [Deep SVDD](http://data.bit.uni-bonn.de/publications/ICML2018.pdf)을 참조 가능
        * nergy-based 방법론의 [Deep structured energy based models for anomaly detection, 2016 ICML]
        * Deep Autoencoding Gaussian Mixture Model 방법론의 [Deep autoencoding gaussian mixture model for unsupervised anomaly detection, 2018 ICLR]
        * Generative Adversarial Network 기반 방법론의 [Anomaly detection with generative adversarial networks, 2018 arXiv]
        * Self-Supervised Learning 기반의 [Deep Anomaly Detection Using Geometric Transformations, 2018 NeurIPS]

<br/>
    
#### Unsupervised
* <U>별도의 labeling이 필요하지 않음</U>

* PCA

* Autoencoder
    * input과 output의 차이에 대한 threshold를 통해서 정상 sample과 비정상 sample을 구분
    * 정상 sample들로 autoencoder를 학습하면, 비정상 sample에 대해서는 잘 동작하지 않기 때문에 output이 이상해지므로 구분 가능

    * <U>하지만, hyper-parameter (e.g. latent variable의 dimension)에 따른 영향이 커서 불안정함</U>
    * <U>또한, input과 output의 차이에 대한 loss func.에 따라서도 성능이 좌지우지됨</U>
    
    > 정상 sample만으로 일단 학습을 진행해야하지 않을까? 이렇게 정상 sample들만 모아 놓는 것 자체가 이미 labeling이랑 뭐가 다르지?



    * 참조 논문
        * [Improving Unsupervised Defect Segmentation by Applying Structural Similarity to Autoencoders](https://arxiv.org/pdf/1807.02011.pdf)
        * [Deep Autoencoding Models for Unsupervised Anomaly Segmentation in Brain MR Images](https://arxiv.org/pdf/1804.04488.pdf)
        * [MVTec AD – A Comprehensive Real-World Dataset for Unsupervised Anomaly Detection]
### References
1. [DEEP LEARNING FOR ANOMALY DETECTION: A SURVEY](https://arxiv.org/pdf/1901.03407.pdf)