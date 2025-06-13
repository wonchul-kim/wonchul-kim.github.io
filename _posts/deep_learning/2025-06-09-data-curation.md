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
    * clustering으로 주어진 데이터셋을 여러 cluster로 분류하고, 각 cluster에서 sampling함으로서 rebalancing하는 것이 목적
    <img src='/assets/deep_learning/data_curation/imagenet.png'>
    <img src='/assets/deep_learning/data_curation/sampling.png'>

* labeling을 하기 이전에 주어진 데이터셋을 정제할 수 있어서 레이블링의 수고를 덜 수도 있음

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

- 기존에는 단순히 clustering을 함으로서 분류하였지만,

- 본 논문에서는 hierarchical clustering + resampling을 함으로서 좀 더 효율적? fair하게? 진행함

    - hierarchical clustering: clustering을 여러 번 하되, 진행한 cluster 내부에서 sub-set을 반복적으로 clustering함

    - 이는 레이블링이 되어 있지 않다는 점에서 하나의 category/class가 여러 cluster에 소속되어 있을 수 있기 때문이라고 생각함

    - 이 때, cluster 내부에서 resampling을 하여 기존의 clustering을 하는 것보다 효율적으로 분류할 point/image/data를 다시 지정함

        - 일반 clustering은 distribution의 중심점을 기준으로 하여 clustering을 하기 때문에 bias가 발생
        
        - 이를 cluster 내부 전체에 대해서 sampling함으로서 bias를 없애고자 함


## References
- [Automatic Data Curation for Self-Supervised Learning: A Clustering-Based Approach](https://arxiv.org/pdf/2405.15613) [code](https://github.com/facebookresearch/ssl-data-curation)