---
layout: post
title: Visual Servoing for precise task
category: Robots
tag: [visual servoing]
---


# Visual Servoing for precise task



* IBVS

* PBVS

## 1. Hybrid Visual Servoing

* 제어 작업을 **파티션(Partition)**으로 나누어, 각 자유도(Degree of Freedom, DOF)나 제어 목표에 따라 **가장 적합한 제어 방식(IBVS 또는 PBVS)**을 독립적으로 적용하는 방식

#### 주요 원리

* 제어 공간의 분할로봇의 6자유도(3축 이동 $\mathbf{t}$와 3축 회전 $\mathbf{r}$)를 제어할 때, 모든 축을 같은 방식으로 제어하는 것이 아니라, 작업에 따라 자유도를 이미지 공간 또는 카르테시안 공간으로 분리

    * 예시 1: 정렬 및 접근 작업
        * Translation (접근): 로봇의 충돌 방지 및 경로 제어가 중요한 이동 방향(예: $Z$축)은 **PBVS (3D 공간 제어)**를 사용
        * Rotation (정렬): 높은 정렬 정확도가 중요한 회전 방향(예: $X, Y$축 회전)은 **IBVS (2D 이미지 공간 제어)**를 사용
        
    * 예시 2: 평면 목표물 추적
        * 목표물의 **평면 내 움직임($X, Y$)**은 IBVS로 제어하여 정확하게 정렬
        * 목표물과의 **거리($Z$)**와 자세는 PBVS로 제어하여 안정적인 거리를 유지
        
#### 장점

* 안정적인 경로 확보: PBVS 제어를 일부 포함함으로써 로봇의 3D 궤적(로봇이 움직이는 경로)이 충돌을 피하고 예측 가능

* 높은 최종 정밀도: IBVS 제어를 포함함으로써 최종 정렬 단계에서 이미지 평면의 픽셀 오차를 최소화하여 높은 정밀도를 달성

## 2. 2.5D 비전 서보잉 (Depth-Aware Visual Servoing)

* 기본적으로 IBVS의 틀을 유지하지만, **깊이 정보(Depth Information)**를 활용하여 IBVS의 주요 단점인 불안정한 궤적 문제를 해결하고 제어 성능을 극대화한 방식

#### 주요 원리

* 깊이 정보의 활용순수 IBVS는 카메라-물체 간의 정확한 깊이($Z$) 정보를 사용하지 않고, 목표 특징과 현재 특징 간의 오차만을 사용

* 여기에 **깊이 추정값 ($\hat{Z}$)**을 활용

* 깊이 정보 추정: 이미지 처리(스테레오 카메라, ToF 센서, 또는 PnP 알고리즘을 통한 간접 추정)를 통해 목표 특징의 깊이 값을 추정

* 상호작용 행렬 개선: 추정된 깊이 값 ($\hat{Z}$)을 사용하여 IBVS 제어 법칙에 사용되는 **상호작용 행렬($\mathbf{L}$)**을 실시간으로 갱신

    * $\mathbf{v}_c = -\lambda \mathbf{L}^+(\hat{Z}) \mathbf{e}$
    * 깊이 $\hat{Z}$가 정확하게 반영되면, $\mathbf{L}$ 행렬이 안정화되어 IBVS의 불안정한 3D 경로 문제를 해결
    
#### 장점

* 궤적 안정성 및 예측 가능성: 깊이 정보를 사용하므로 로봇의 3D 궤적이 목표 지점을 향해 곧게 수렴하도록 설계 (PBVS의 장점 일부 수용)
* IBVS의 강건성 유지: 제어 오차의 주된 입력은 여전히 **이미지 특징 오차($\mathbf{e}$)**이기 때문에, PBVS에 비해 카메라 캘리브레이션 오차나 기구학 모델 오차에 대한 강건성을 유지


## References

#### Visual servo control. I. Basic approaches (F. Chaumette, S. Hutchinson)

#### Visual servo control. II. Advanced approaches