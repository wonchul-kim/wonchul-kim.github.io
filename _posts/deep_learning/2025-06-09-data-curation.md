---
layout: post
title: Automatic Data Curation for Self-Supervised Learning
category: Deep Learning
tag: [reduce dataset]
---

# [Automatic Data Curation for Self-Supervised Learning: A Clustering-Based Approach](https://arxiv.org/pdf/2405.15613)




## Approach

### A criterion for creating pre-training datasets 


* dataset의 imbalance는 성능에 영향을 미친다.
    <img src='/assets/deep_learning/data_curation/imagenet.png'>

* we can imitate this process by dividing the data pool into clusters using methods such as k-means (Lloyd, 1982;
Arthur & Vassilvitskii, 2007) and considering the cluster index as a proxy category. We will discuss the limitations of this approach in the next section. In this work, we propose a more general approach: sampling data points from the uniform distribution over the support of the data distribution. A subset obtained that way is asymptotically balanced concept-wise if the embedding space in which the distribution is manipulated is well organized. In such space, data points that are semantically more similar lie close to each other, or in other words, the induced metric distance reflects the “semantic distance”. For example, in an embedding space where concepts are represented by non-overlapping blobs of equal size, our proposed sampling approach is equivalent to sampling the same number of points from each concept.


### Rebalancing datasets with k-means

* 하나의 `concept` (해당 이미지의 class/category를 의미)는 여러 개의 cluster로 나누어질 수 있는데, 이는 하나의 `concept`이 하나의 cluster로 대변되는 것을 방지한 것이다. 예를 들어, imbalanced dataset에 대해서 하나의 dominant class를 하나의 cluster로 표현하게 되면, 해당 cluster는 너무 많은 데이터 sample을 갖게 된다. 그리고 이는 나중에 uniform distribution으로 sampling할 때, 문제가 되므로 cluster들이 동일한 수의 data를 갖는 것이 좋다. 

    * 그래서 **k-means**는 동일한 `concept`이더라도 이를 여러 개의 cluster로 나누어질 수 있도록 한다. 

    * 하지만, 여전히 sampling할 때에 각 cluster마다 동일한 sampling을 하게 되면 여전히 imbalance가 존재하기 때문에 **hierarchical k-means**를 본 논문에서 소개한다.

    * Results from Zador (1982; 1964) provide another explanation to the phenomenon. It turns out that in $d$ dimension, the k-means centroids asymptotically follow the distribution with density proportional to $p^{d/(d+2)}$.
        * 차원이 증가할수록 중심점 분포가 원본 데이터 분포에 가까워진다는 의미로, 차원의 저주와 관련된 현상으로, 고차원에서는 밀집 영역에 클러스터가 집중되는 경향이 강해집니다.


* It means that k-means forms significantly more clusters in higher-density areas in the
embedding space, which correspond to dominant concepts.


### Rebalancing datasets with hierarchical k-means



## References
- [Automatic Data Curation for Self-Supervised Learning: A Clustering-Based Approach](https://arxiv.org/pdf/2405.15613) [code](https://github.com/facebookresearch/ssl-data-curation)