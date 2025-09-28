---
layout: post
title: Active Learning
category: Deep Learning
tag: [active learning]
---


# Active Learning 

Deep Active Learning (DeepAL) has emerged from whether AL can be used to reduce the cost of sample annotation
while retaining the powerful learning capabilities of DL.

AL is just such a method dedicated to studying how to obtain as many performance gains as possible by labeling as few samples as possible.

More specifically, it aims to select the most useful samples from the unlabeled dataset and hand it over to the oracle (e.g., human annotator) for labeling, to reduce the cost of labeling as much as possible while still maintaining performance.


## [A Survey of Deep Active Learning](https://arxiv.org/pdf/2009.00236)

#### membership query synthesis 

#### stream-based selective sampling

#### pool-based AL


## [ACTIVE LEARNING FOR CONVOLUTIONAL NEURAL NETWORKS: A CORE-SET APPROACH](https://arxiv.org/pdf/1708.00489)

Empirically, many of the active learning heuristics in the literature are not effective when applied to CNNs in batch setting. Inspired by these limitations, we define the problem of active learning as core-set selection, i.e. choosing set of points such that a model learned over the selected subset is competitive for the remaining data points.


## References

- [A Survey of Deep Active Learning](https://arxiv.org/pdf/2009.00236)
- [ACTIVE LEARNING FOR CONVOLUTIONAL NEURAL NETWORKS: A CORE-SET APPROACH](https://arxiv.org/pdf/1708.00489)