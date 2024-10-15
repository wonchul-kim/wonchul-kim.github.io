---
layout: post
title: Open-World Semantic Segmentation Including Class Similarity
category: Computer Vision
tag: [open-world segmentation]
---

This paper investigates the problem of open-world semantic segmentation. Given an image at test time, we aim
to have a model that is able to detect any pixel that belongs to a category that was unseen at training time and is
also able to distinguish between different new categories.
The first problem is called anomaly segmentation [9] and
aims to achieve a binary segmentation between known and
unknown. The second problem, called novel class discovery [21], aims to obtain a pixel-wise classification of novel
samples into different classes starting from the knowledge
of previously seen, labeled samples.


extended from
*  Abhijit Bendale and Terrance E. Boult. Towards open set
deep networks. In Proc. of the IEEE/CVF Conf. on Computer
Vision and Pattern Recognition (CVPR), 2016. 2, 3

* Petra Bevandic, Ivan Kreso, Marin Orsi ´ c, and Sinisa Segvi ´ c.´
Simultaneous semantic segmentation and outlier detection in
presence of domain shift. Pattern Recognition, 11824:33–47,
2019. 2

* Xincheng Yao, Ruoqi Li, Jing Zhang, Jun Sun, and
Chongyang Zhang. Explicit Boundary Guided Semi-PushPull Contrastive Learning for Supervised Anomaly Detection. In Proc. of the IEEE/CVF Conf. on Computer Vision
and Pattern Recognition (CVPR), 2023. 2



----- 

papers to read

* NonBottleneck-1D block: Eduardo Romera, Jose M. Alvarez, Luis M. Bergasa, and ´
Roberto Arroyo. Erfnet: Efficient residual factorized convnet for real-time semantic segmentation. IEEE Trans. on
Intelligent Transportation Systems (T-ITS), 19(1):263–272,
2018. 3, 2

* swiftnet: Marin Orsic, Ivan Kreso, Petra Bevandi ´ c, and Sinisa Segvi ´ c.´
In Defense of Pre-Trained ImageNet Architectures for RealTime Semantic Segmentation of Road-Driving Images. In
Proc. of the IEEE/CVF Conf. on Computer Vision and Pattern Recognition (CVPR), 2019. 3

* leverages the contrastive loss [11]
* objectosphere loss [15] 

-----

* Hadamard product 

---- 

The aim of semantic segmentation is to predict a categorical distribution over  classes for all pixels in an image.

----
