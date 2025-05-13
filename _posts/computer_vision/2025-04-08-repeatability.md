---
layout: post
title: Repeatability
category: Computer Vision
tag: [repeatability]
---


### Factors that hinder repeatability in deep learning

- Translation 

    - `shift equivalence` in deep learning

    - **CNN**은 `shift equivalence`에 취약함
        - stride가 2 이상인 pooling이나 convolution에 의해서 위치 정보가 손실
        - nonlinear activation에 의해서 입력에서의 위치가 변하면 완전히 다른 출력을 생성할 수 있음
        - interpolation, upsampling, 특히 bilinear 등은 위치 정합성을 보장하지 않음
        - aliasing으로 인해서 고주파수 정보가 손실될 수 있음


    - [Investigating Shift Equivalence of Convolutional Neural Networks in Industrial Defect Segmentation](https://arxiv.org/pdf/2309.16902) [code](https://github.com/xiaozhen228/CAPS)


    - [Learnable Polyphase Sampling for Shift Invariant and Equivariant Convolutional Networks](https://arxiv.org/pdf/2210.08001)

    - [Making Convolutional Networks Shift-Invariant Again](https://arxiv.org/pdf/1904.11486)

    - [Improving Shift Invariance in Convolutional Neural Networks with Translation Invariant Polyphase Sampling](https://openaccess.thecvf.com/content/WACV2025/papers/Saha_Improving_Shift_Invariance_in_Convolutional_Neural_Networks_with_Translation_Invariant_WACV_2025_paper.pdf)

    - [Alias-Free Convnets](https://arxiv.org/pdf/2303.08085)

    - [On the effectiveness of Rotation-Equivariance in U-Net: A Benchmark for Image Segmentation](https://arxiv.org/pdf/2412.09182)

    - [Lost in Translation: Modern Neural Networks Still Struggle With Small Realistic Image Transformations](https://arxiv.org/pdf/2404.07153)

- Color

    - `robustness to appearance varations`
    
    - `generalization to pertuabations`

    - `invaraince to illumination`

- Noise


### Measurements

- Dice similarity coefficient (DSC)

    - has several limitations to consider when interpreting its results.

- Surface Dice coefficient (SDC)

- Hausdorf distance (HD)

- Concordance Correlation Coefficient (CCC)

- Intraclass Correlation Coefficient (ICC)


- [Radiomics as a measure superior to the Dice similarity coefficient for tumor segmentation performance evaluation](https://arxiv.org/pdf/2310.20039)


### References

- [SAM2-Adapter: Evaluating & Adapting Segment Anything 2 in Downstream Tasks: Camouflage, Shadow, Medical Image Segmentation, and More](https://arxiv.org/pdf/2408.04579) [code](https://github.com/tianrun-chen/SAM-Adapter-PyTorch/tree/SAM2-Adapter-for-Segment-Anything-2?tab=readme-ov-file)

- [Generative Adversarial Networks to Improve the Robustness of Visual Defect Segmentation by Semantic Networks in Manufacturing Components](https://www.mdpi.com/2076-3417/11/14/6368?utm_source=chatgpt.com)

- [Reliability in Semantic Segmentation: Are We on the Right Track?](https://arxiv.org/pdf/2303.11298)

### etc.

- [Improving the repeatability of deep learning models with Monte Carlo dropout](https://arxiv.org/pdf/2202.07562)