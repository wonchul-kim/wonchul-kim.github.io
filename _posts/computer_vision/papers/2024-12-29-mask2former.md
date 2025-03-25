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

> We hypothesize that local features are enough to update query features and context information can be gathered through self-attention.




#### Multi-scale strategy 

- feeds successive feature maps from the pixel decoder's feature pyramid into successive transformer decoder layers in a round robin fashion.

    - round robin fashion: each transformer layer gets a different resolution feature in a cyclic manner
        - e.g.) layer1: high, layer2: low, layer3: high, layer4: low, ...


    



## References:

- [Masked-attention Mask Transformer for Universal Image Segmentation](https://arxiv.org/pdf/2112.01527)