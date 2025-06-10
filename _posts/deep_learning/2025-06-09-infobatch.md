---
layout: post
title: INFOBATCH
category: Deep Learning
tag: [reduce dataset]
---

# [INFOBATCH: LOSSLESS TRAINING SPEED UP BY UNBIASED DYNAMIC DATA PRUNING](https://arxiv.org/pdf/2303.04947)

*  filter out samples that make less contribution to the training

* **InfoBatch**, a novel framework aiming to achieve lossless training acceleration by unbiased dynamic data pruning. Specifically, <U>InfoBatch randomly prunes a portion of less informative samples based on the loss distribution and rescales the gradients of the remaining samples to approximate the original gradient.</U> 

* As a plug-and-play and architecture-agnostic framework, InfoBatch consistently obtains lossless training results on classification, semantic segmentation, vision pertaining, and instruction fine-tuning tasks. 

## Introduction

#### to reduce the training sample amount

* Dataset distillation
* Coreset selection
* Weighted sampling method
* LARS and LAMB ?????????????????????????????????//
    * [LARGE BATCH TRAINING OF CONVOLUTIONAL NETWORKS](https://arxiv.org/abs/1708.03888)
    * [LARGE BATCH OPTIMIZATION FOR DEEP LEARNING: TRAINING BERT IN 76 MINUTES](https://arxiv.org/abs/1904.00962)

* Data pruning
    * Static data pruning: pruned samples are never back during training
        * [An Empirical Study of Example Forgetting during Deep Neural Network Learning](https://arxiv.org/abs/1812.05159)
        * [Deep Learning on a Data Diet: Finding Important Examples Early in Training](https://arxiv.org/abs/2107.07075)

        * There are 2 problems:
            * It takes additional time to estimate more accurate scores 
            * Pruning directly the samples cuase gradient expectation bias

* This paper is about dynamic data pruning

    * To tackle the issues of static data pruning, InfoBatch, a novel unbiased dynamic data pruning framework based on the idea of maintaining the same expected total update between training on the pruned and original datasets, is proposed. 
        * expectation rescaling. 
        * given a dataset, maintain a score of each sample with its loss value during forward propagation
        * randomly prune a certain portion of  small-score (i.e. welllearned) samples in each epoch
        * Different from previous methods where well-learned samples are dropped directly, it scales up the gradient of those remaining smallscore samples to  keep an approximately same gradient expectation as the original dataset.


## How we can achieve lossless result

#### Dynamic pruning

* There are two steps:
    1. soft pruning: randomly prunes some samples from $D_1$ with relatively small scores
    2. expectation rescaling: For remaining samples from $D_1$, expectation rescaling scales up the losses to keep up the approximately same gradient expectation as the original dataset



## References
- [INFOBATCH: LOSSLESS TRAINING SPEED UP BY UNBIASED DYNAMIC DATA PRUNING](https://arxiv.org/pdf/2303.04947)[code](https://github.com/NUS-HPC-AI-Lab/InfoBatch)