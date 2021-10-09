---
layout: post
title: For small objects
category: Computer vision
tag: small objects
---

# To improve performance on small objects

당연한 이야기지만, 사람에게 있어서도 작은 물체는 큰 물체보다 detection하기 힘들다. 같은 이유라고 할 수는 없지만, 동일한 맥락에서 Computer vision algorithm에서도 작은 물체는 큰 물체보다 detection 성능이 떨어진다. 예를 들어서, EfficientDet을 활용하여 작은 물체에 대해서는 mAP가 약 12%이고, 큰 물체는 약 51%의 성능을 보인다.  

이를 좀 더 이론적으로 살펴보면, 일반적인 computer vision algorithm은 여러 층의 CNN을 통해서 feature를 세세하게 추출해나감으로서 주어진 image를 분석 및 해석한다. 이 때, 작은 물체의 feature는 추출하여 마지막까지 남아 설계된 loss function (대부분은 multi-task loss func.)의 계산을 통해서 model의 weight에 영향을 미치기 힘들다. 그리고 작은 물체의 경우에는 필연적으로 ground truth도 작기 때문에 loss func.에서 미치는 영향이 작을 수밖에 없다. 

이를 해결하기 위해서는 무엇을 해야할까!?

## Increase image capture resolution

* 작은 물체는 이미지에서 얼마 되지 않는 pixel로 구성된다. 이 때, pixel을 늘리면 feature를 추출할 수 있는 pixel의 수가 늘어나기 때문에 도움이 될 수 있다. 하지만, 이는 원본 image의 resolution이 커야한다는 것이기 때문에 실용적인 해결책은 아니라고 생각한다. 

> 기존의 image의 resolution을 좀 더 높은 resolution으로 만드는 방법이 실용적이지 않을까!?

## Increase model's input resolution

* model의 input image size는 작은 물체를 늘리는 것보다는 큰 물체를 작게 하여 하는 것이 좋다. 

* image의 resolution이 model's input resolution보다 크다면, 기존 image의 aspect ratio를 유지하면서 줄이는 게 일반적으로 더 낫다.

## Tile images

## Generate more data via augmentation

* [random crop](https://blog.roboflow.com/why-and-how-to-implement-random-crop-data-augmentation/)
* [random rotation](https://blog.roboflow.com/why-and-how-to-implement-random-rotate-data-augmentation/)
* [mosaic augmentation](https://blog.roboflow.com/advanced-augmentations/)

## Auto learn model anchors

## Filter out extraneous classes