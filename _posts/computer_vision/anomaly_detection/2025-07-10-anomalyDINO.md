---
layout: post
title: AnomalyDINO
category: Deep Learning
tag: [reduce dataset]
---

# [AnomalyDINO: Boosting Patch-based Few-shot Anomaly Detection with DINOv2](https://arxiv.org/pdf/2405.14529)

* patchlevel deep nearest neighbor paradigm
* enables both image-level anomaly prediction and pixel-level anomaly segmentation

* The approach is methodologically simple and training-free and, thus, does not require any additional data for fine-tuning or meta-learning

* The reduced overhead, coupled with its outstanding few-shot performance, makes AnomalyDINO a strong candidate for fast deployment


## 1. Intro.

* **Anomaly detection (AD)** in machine learning attempts to identify instances that deviate substantially ***from the nominal data distribution pnorm(x)***

> Our approach, termed AnomalyDINO, follows the wellestablished AD framework of patch-level deep nearest neighbor [34, 46], and leverages DINOv2 [30] as a backbone. We carefully design a suitable preprocessing pipeline for the few-shot scenario, using the zero-shot segmentation abilities of DINOv2 (which alleviates the additional overhead of another segmentation model). At test time, anomalous samples are detected based on the high distances between their patch representations and the closest counterparts in the nominal memory bank M.


> The performance of few-shot anomaly detection techniques has already been boosted by the use of foundation models, mostly by multimodal approaches incorporating language and vision [4, 19, 21, 50].

## 2. Related Works

#### Foundation Models for Vision

* **DINOv2** combines ideas from **DINO** with *patch-level reconstruction techniques* and scales to larger architectures and datasets.

* **GroundingDINO**, builds upon the DINO framework and focuses on improving the alignment of textual and visual information, enhancing the model’s performance in tasks requiring detailed object localization and multimodal understanding.


#### Anomaly Detection

> do not target the detection of *semantic anomalies* but of *low-level features* such as scratches of images of industrial products. Several works tackled this task by either training an anomaly classifier [38] or a generative model, which allows for reconstruction-based or likelihood-based AD [9, 26, 28, 42, 48].

> A well-established approach is that of **deep nearest neighbor AD**, where test instances are scored according to
the distance to their nearest neighbors (in feature space) from the *memory bank M*, containing features of nominal
instances. 

* **PatchCore** utilizes pre-trained classification models on Imagenet for patch-level feature extraction. 

* Another line of work builds upon the success of pre-trained language-vision models in zero-shot classification. 
    * First, these approaches define sets of prompts describing nominal samples and anomalies. 
    * Second, the corresponding textual embeddings are compared against the image embeddings. Images whose visual embedding is close to the textual embedding of a prompt associated with an anomaly are classified as anomalous. * owever, these methods either require significant prompt engineering or fine-tuning of the prompt(-embeddings).


> another type of few-shot anomaly detection builds upon the success of multimodal chatbots. These methods
require more elaborate prompting and techniques for interpreting textual outputs. Since these methods do not require a memory bank, they are capable of performing zeroshot anomaly detection.

## 3. Matching Patch Representations for Visual Anomaly Detection


* by tailoring the memory bank concept to the few-shot regime, 
    * specifically utilizing the strong features from DINOv2: We design a pipeline that incorporates zero-shot masking and augmentations
    * propose a more robust aggregation statistic

#### 3.1 Matching Patch Representations for Visual Anomaly Detection

> Our work differs from previous deep nearest neighbor approaches [34, 46] by tailoring the memory bank concept
to the few-shot regime, specifically utilizing the strong features from DINOv2: We design a pipeline that  incorporates zero-shot masking and augmentations, and we propose a more robust aggregation statistic. Importantly, we identify DINOv2 as the ideal backbone for our scenario due to its strong patch-level features and masking ability. In addition, this simplifies the deep nearest neighbor framework by reducing the complexity of the feature engineering stage (e.g., by making it unnecessary to think of which representations/layers to use and how to aggregate them), and increasing the flexibility w.r.t. input resolution (that can be chosen to be any multiple of 14). The proposed method is described in detail in the following subsections.


#### 3.2. Enriching the Memory Bank & Filtering Relevant Patches

> To avoid irrelevant areas of the test image from leading to falsely high anomaly scores, we propose masking the object of interest. This approach mitigates the risk of false positives, particularly in the few-shot regime where the limited reference samples may not adequately capture the natural variations in the background

> Masking, i.e., delineating the primary object(s) in an image from its background, can help to reduce falsepositive predictions, thereby improving the robustness in the low-data regime. To minimize the overhead of the proposed pipeline, we utilize DINOv2 also for masking. This is achieved by thresholding the first PCA component of the patch features [30]. We observe empirically that this sometimes produces erroneous masks. Frequently, such failure cases occur for close-up shots, where the objects of interest account for ≳ 50% of patches. To address this issue, we check if the PCA-based mask accurately captures the object in the first reference sample and apply the mask accordingly, which gives rise to the ‘masking test’ in Figure 2. This test is performed only once per object as the procedure yields very consistent outputs. In addition, we utilize dilation and morphological closing to eliminate small holes and gaps within the predicted masks. See Figures 14 and 15 for examples of this masking procedure, Table 8 for the outcomes of the masking test per object, and Figure 10 for a visualization of the benefits of masking in the presence of background noise (all in Appendix C). In general, we do not mask textures (e.g., ‘Wood’ or ‘Tile’ in MVTec-AD).

## References
- [AnomalyDINO: Boosting Patch-based Few-shot Anomaly Detection with DINOv2](https://arxiv.org/pdf/2405.14529) [code](https://github.com/dammsi/AnomalyDINO)