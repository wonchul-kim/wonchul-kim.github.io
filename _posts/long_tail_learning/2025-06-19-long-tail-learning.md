---
layout: post
title: Long-tail Learning
category: Long-tail Learning
tag: [long-tail learning]
---

# Long-tail Learning

* 클래스 불균형: 일반 객체(사람, 차, 개)는 수천 장, 희귀 객체(기린, 드론 등)는 수십 장

* 인스턴스 불균형: 이미지 1장에 여러 개 등장하는 head 클래스 vs. 드물게 나오는 tail 클래스

* 컨텍스트 편향: tail 클래스는 특정 배경이나 위치에서만 등장 → 과적합 우려

* 예: COCO, LVIS 등에서 빈도 상위 10개 클래스가 전체 인스턴스의 대부분을 차지함



* 기존의 Faster R-CNN, RetinaNet, YOLO 계열은 다음 이유로 tail class에 불리합니다:

    | 원인                 | 설명                                             |
    | ------------------ | ---------------------------------------------- |
    | Softmax classifier | head class의 gradient가 대부분을 차지함                 |
    | NMS bias           | confidence가 높은 head class만 살아남음                |
    | IoU 기반 loss        | tail class는 학습 signal이 약해서 box refinement도 불리함 |


## 해결하기 위한 접근법들

#### Re-sampling (data-level)

* Repeat factor sampling (RFS): tail class가 포함된 이미지를 더 자주 보여줌

#### Re-weighting (loss-level)

* class-balanced loss (CB loss): 클래스 수에 따라 손실 함수에 가중치 부여

* Equalization Loss (EQL, EQLv2):

    * tail class의 gradient만 유지

    * head class의 gradient는 일정 확률로 차단

    * Softmax 분류기 문제 보정


#### Decoupled Learning (Representation ↔ Classifier)

* classifier만 따로 재학습 

    * Representation은 Freeze

* "Decoupled Classification Refinement" (CVPR 2020)
    
    * 1단계는 Representation 학습: 전체 데이터를 기반으로 backbone + ROI feature extractor를 전체 class에 대해서 학습

    * 2단계는 Classifier 재학습: backbone을 freeze한 뒤, tail class에 맞춰 classifier만 새로 학습
        
        * 단, tail class의 데이터가 적으므로, 보통 아래 중 하나를 적용:

            * 전체 데이터로 재학습 + class re-weighting (tail class에 가중치 부여)

            * tail 클래스 샘플만 모아서 재학습 (rare class fine-tuning)

            * 혹은 hard negative mining으로 tail class 및 어려운 샘플 집중 학습

            * DCR 논문에서는 tail만 따로 떼서 학습한다기보다는 전체 클래스에 대해 classifier만 재학습하는 방식에 가까움

    | 단계  | 학습 범위                                                          | 입력                                                   | 출력            | Loss                                           |
    | --- | -------------------------------------------------------------- | ---------------------------------------------------- | ------------- | ---------------------------------------------- |
    | 1단계 | Backbone + ROI Extractor + Classifier (전체 detector end-to-end) | 이미지                                                  | 클래스 확률 + bbox | Classification + bbox regression loss (전체 클래스) |
    | 2단계 | Classifier만 재학습 (Backbone, ROI extractor freeze)               | ROI feature (이미지 → backbone → ROI extractor 통과 후 결과) | 클래스 확률        | Classification loss (전체 클래스 or tail 가중치 적용 가능) |



* Head class의 overfitting을 막고, tail class를 더 섬세하게 학습시킬 수 있음.


## 데이터셋

| 데이터셋           | 특징                                  |
| -------------- | ----------------------------------- |
| **LVIS**       | COCO 기반, 1200+ 클래스, 심한 long-tail 분포 |
| **COCO-LT**    | COCO 데이터에서 long-tail 형태로 필터링        |
| **OpenImages** | 매우 다양한 클래스와 long-tail 분포            |
| **TaFe-LT**    | CLIP 기반 Tail-Focused 데이터셋 (연구용)     |


## 성능지표
* mAP@50, mAP@75: 일반 mAP는 head class가 지배

* 따라서, mAP-all, mAP-head, mAP-medium, mAP-tail을 모두 분류해서 분석

