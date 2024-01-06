---
layout: post
title: Augmentations for Object Detection
category: Computer Vision
tag: augmentation
---

일반적인 딥러닝 라이브러리에서 제공하는 augmentation function이외에 실제로 data를 직접 augmentation하는 과정에서는 
이미지뿐만 아니라 이미지의 정보가 담긴 annotation(object location coordinates)까지 고려해야한다. 

이에 대해서 다음의 코드가 매우 도움이 된다. 

https://github.com/Paperspace/DataAugmentationForObjectDetection/blob/master/quick-start.ipynb
