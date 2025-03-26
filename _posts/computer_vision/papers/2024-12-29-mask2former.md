---
layout: post
title: Mask2Former
category: Computer Vision
tag: [maskformer]
---


# [Masked-attention Mask Transformer for Universal Image Segmentation](https://arxiv.org/pdf/2112.01527)

- architecture
    - backbone feature extractor
        - extract low-resolution features
    - pixel decoder 
        - gradually upsample low-resolution features to generate high-resolution per-pixel embeddings
    - **transformer decoder (main contribution)**
        - operates on image features to process object queries
        - the final binary mask predictions are decoded from per-pixel embeddings with object queries

1. masked attention in transformer decoder 
    - restricts the attention to localized features centered around predicted segmentation, which can be either objects or regions depending on the specific semantic for grouping
    - Compared to the cross-attention used in a standard Transformer decoder which attends to all locations in an image, our masked attention leads to faster convergence and improved performance

2. multi-scale high-resolution features 
    - to segment small object/regions

3. optimization improvements
    - switching the order of self and cross-attention
    - making query features learnable
    - removing dropout

4. save 3xtraining memory w/o affecting the performance by calculating mask loss on few randomly sampled points






### 3.1 Mask Classificatoin preliminaries

The meta architecture is MaskFormer replaced by **Transformer decoder**.

> However, the challenge is to find good representations for each segment. For example, Mask RCNN [24] uses bounding boxes as the representation which limits its application to semantic segmentation. Inspired by DETR [5], each segment in an image can be represented as a C-dimensional feature vector (“object query”) and can be processed by a Transformer decoder, trained with a set prediction objective.

### 3.2 Transformer decoder with masked attention

#### Masked attention

- extracts localized features by constraining cross-attetion to within the aforeground region of the predicted mask for each query, instead of attending to the full feature map.

    - aforeground region: the region each query predicted where the object to detect locates

> Context features have been shown to be important for image segmentation [7,8,63]. 

- However, recent studies [22,46] suggest that the slow convergence of Transformer-based models is due to global context in the cross-attention layer, as it takes many training epochs for cross-attention to learn to attend to localized object regions [46].

- We hypothesize that local features are enough to update query features and context information can be gathered through self-attention. For this we propose masked attention, a variant of crossattention that only attends within the foreground region of the predicted mask for each query.

- standard cross-attention w/ residual path
$$
X_l = \text{softmax}(Q_l K_l^T) V_l + X_{l-1}
$$

where:  
- $X_l$ : Output of the $l$-th Transformer decoder layer
    - $X_l \in \mathbb{R}^{N \times C}$
    - $N$ $C$-dimensional query features
    - $X_0$: input query features to the transformer decoder
- $Q_l$ : Query at layer $ l $  
    - $Q_l = f_{Q}(X_{l-1}) \in \mathbb{R}^{N \times C}$
- $K_l$ : Key at layer $l$ under transformer $f_{K}(\cdot)$
    - $K_l \in \mathbb{R}^{H_{l}W_{l} \times C}$
    - $H_{l}, W_{l}$: spatial resolution of image features
- $V_l$ : Value at layer $l$ under transformer $f_{V}(\cdot)$
    - $V_l \in \mathbb{R}^{H_{l}W_{l} \times C}$
- $X_{l-1} $ : Output from the previous layer ($ l-1 $)  
- $\text{softmax}(Q_l K_l^T) $ : Attention mechanism that computes the importance of different feature locations


- mask attention
$$
X_l = \text{softmax}(M_{l-1} + Q_l K_l^T) V_l + X_{l-1}
$$



#### Multi-scale strategy 

- feeds successive feature maps from the pixel decoder's feature pyramid into successive transformer decoder layers in a round robin fashion.

    - round robin fashion: each transformer layer gets a different resolution feature in a cyclic manner
        - e.g.) layer1: high, layer2: low, layer3: high, layer4: low, ...

- 1/32, 1/16, 1/8
- sinusoidal poisional embedding + learnable scale-level embedding
    
> how is pixel decoder implemented???????????????????????????


#### Transformer Decoder 
A standard Transformer decoder layer [51] consists of three
modules to process query features in the following order: a
self-attention module, a cross-attention and a feed-forward
network (FFN). Moreover, query features (X0) are zero initialized before being fed into the Transformer decoder and
are associated with learnable positional embeddings. Furthermore, dropout is applied to both residual connections
and attention maps.

To optimize the Transformer decoder design, we make
the following three improvements. First, we switch the order of self- and cross-attention (our new “masked attention”) to make computation more effective: query features
to the first self-attention layer are image-independent and
do not have signals from the image, thus applying selfattention is unlikely to enrich information. Second, we
make query features (X0) learnable as well (we still keep
the learnable query positional embeddings), and learnable
query features are directly supervised before being used in
the Transformer decoder to predict masks (M0). We find
these learnable query features function like a region proposal network [43] and have the ability to generate mask
proposals. Finally, we find dropout is not necessary and
usually decreases performance. We thus completely remove
dropout in our decoder.


#### randomly sampled calculation of mask loss 

Motivated by PointRend [30] and
Implicit PointRend [13], which show a segmentation model
can be trained with its mask loss calculated on K randomly
sampled points instead of the whole mask, we calculate the
mask loss with sampled points in both the matching and
the final loss calculation


More specifically, in the matching loss that constructs the cost matrix for bipartite matching, we uniformly sample the same set of K points for all
prediction and ground truth masks. In the final loss between predictions and their matched ground truths, we sample different sets of K points for different pairs of prediction and ground truth using importance sampling [30]. We
set K = 12544, i.e., 112 × 112 points. This new training
strategy effectively reduces training memory by 3×, from
18GB to 6GB per image


## References:

- [Masked-attention Mask Transformer for Universal Image Segmentation](https://arxiv.org/pdf/2112.01527)