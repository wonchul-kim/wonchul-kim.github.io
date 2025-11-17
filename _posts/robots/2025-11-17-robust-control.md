---
layout: post
title: Robust Control for Visual Servoing using Manipulator
category: Robots
tag: [robust control, control, visual servoing, manipulator]
---


# Manipulator를 활용한 Visual Servoing에서 Robust Control의 역할

## **assembly**은 카메라로 객체를 인식하고, 로봇팔이 객체에 다가가는 것이다. 이 때, 객체를 인식하는 것은 **Computer Vision**이고, **Robust Control**은 어떤 역할을 하는가?

1. 모델 오차(Hand–Eye, Jacobian, depth) → 발산 방지

2. 노이즈·미세 tracking 오류 → 제어 진동(oscillation) 억제

3. 접촉/삽입 단계의 충격과 마찰 → 안정성을 유지 (robust control 없으면 삽입 직전에 자세가 흔들림)

4. 로봇 동역학 불확실성 → 정확하게 추종되도록 보정 (모터/기어박스/마찰은 항상 모델과 다름)

5. 지연·저주기 VS의 구조적 취약성 보완



### 1. 모델 오차 보정 — Hand–Eye / Jacobian / Depth 오차에 대해 안정성 유지

#### 어떤 문제가 생기나?

* 카메라–로봇 팔 간 변환(Hand–Eye) 오차

* 이미지 Jacobian 불확실성 (특히 IBVS)

* Depth, scale 오차

* 로봇 동역학 파라미터 오차 (질량/관성/마찰)

VS는 정확한 모델을 요구하는데 조금만 틀려도 실제 로봇 동작은 divergence(발산)하거나, 목표 주변에서 진동하고, 오차가 줄지 않는 문제가 생김.

#### robust control이 하는 일

* 모델이 틀려도 수렴성(안정성)을 보장

* Hand–Eye가 정확하지 않아도 end-effector가 목표로 수렴하도록 함

* Jacobian이나 depth가 틀려도 tracking error가 폭주하지 않게 억제

* Adaptive / SMC / H∞ 제어기 등이 이 영역에 강함

#### 조립에서 이것이 왜 중요?

* 삽입 위치가 조금만 틀려도 조립이 실패

* 카메라의 pose calibration이 완벽할 수 없음 → robust control이 이를 보정해주는 셈

### 2. 노이즈·트래킹 jitter 억제 — 영상 기반 제어 특유의 떨림·고주파 입력을 안정화

#### 어떤 문제가 생기나?

* 카메라 영상에서 피처는 항상 미세하게 흔들립니다:

* 조명 변화

* subpixel feature jitter

* 이미지 노이즈

* tracking 알고리즘의 불안정성

* 카메라 프레임이 30–60Hz라 제어가 “저주파/지연” 구조

이 작은 noise들이 제어 입력에 직접 전달되면 제어기와 로봇 팔이 떨리고, 조립 작업에 치명적입니다.

#### robust control이 하는 일

* 고주파 이미지 노이즈가 제어 출력으로 전달되지 않도록 차단

* tracking jitter로 인해 생기는 oscillation 억제

* sliding mode의 chattering 제거, H∞의 noise attenuation 등

* MPC는 constraint를 통해 입력을 스무딩

즉, VS의 noisy input → smooth한 모션으로 변환하는 장치.

#### 조립에서 이것이 왜 중요?

* 조립 단계에서는 end-effector가 0.1~0.5mm 단위로 정렬해야 하는데, 노이즈가 그대로 로봇에 전달되면 → 홀 입구에서 팔이 미세하게 흔들려 삽입 실패.

### 3. 접촉 / 외란 안정성 — Assembly에서 발생하는 충격, 마찰, 구속력에 대해 제어가 무너지지 않도록 함

#### 어떤 문제가 생기나?

* 조립은 필연적으로 contact task입니다.

* 물체 표면 접촉

* 구멍 입구 edge 접촉

* 삽입/맞물림 중 side friction

* 동역학적 외란(접촉 순간 충격력 spike)

Visual servoing은 본질적으로 비접촉 기반의 위치 제어기이기 때문에 접촉 순간에는 VS의 모델 가정이 붕괴합니다.


#### robust control이 하는 일

* 접촉 충격에도 시스템 안정성 유지

* momentary disturbance를 빠르게 감쇄(damping)

* 외란을 모델링/추정(DOB)하여 제어 입력에서 보상

* sliding mode는 lumped disturbance rejection에 강함

* H∞는 external disturbance에 대한 L2 gain을 제한

즉, 접촉이 있는 환경에서도 VS 기반 정렬이 안정적으로 유지되게 해줌.

#### 조립에서 이것이 왜 중요한가?

* 삽입 시:

    * 약간의 충격으로 pose가 바뀌면 → 삽입 실패

    * 마찰이 생기면 trajectory tracking이 틀어짐

    * contact dynamics가 로봇을 튕기면 → oscillation → 실패


| 영역               | VS에서 생기는 문제                           | Robust control의 역할                 |
| ---------------- | ------------------------------------- | ---------------------------------- |
| **A. 모델 오차 보정**  | Hand–Eye, Jacobian, depth 오차 → 발산/불안정 | 모델 불확실성에 robustness 확보, 안정 수렴 유지   |
| **B. 노이즈/진동 억제** | tracking jitter → 제어 떨림               | noise attenuation, smooth input 생성 |
| **C. 접촉/외란 안정성** | 충격·마찰 → pose 튐/불안정                    | 외란 억제, contact 안정성 확보              |

-------------------------------------------

## Robust Control에서 "오차"를 보정한다고 하는데, 이 "오차"란 무엇이며 보정해서 결론적으로 어떻게 하겠다는 것인가?

> 오차를 보정한다는 건, 예를 들어서, robust control 이론으로 다음으로 위치해야할 각 joint의 값을 output으로 계산했는데 실제로 움직인 다음의 현재 joint와 생성한 joint의 값의 차이를 오차로 이용하는걸까요? 그렇다면, 결론적으로 이 오차를 이용해서 다음 step의 output을 어떻게 계산하겠다는 걸까요?

> YES, 이것도 오차(“tracking error”) 중 하나입니다. 그러나 robust control에서 중요한 오차의 종류는 3가지입니다.

#### 1. Tracking Error

* Visual servoing에서 이미지 상에서의 오차 
    $$ e = s − s^* $$

    * $s$ = 현재 이미지 피처

    * $s^*$ = 목표 이미지 피처

* PBVS라면,
$$ e = pose_{current} − pose_{desired} $$


#### 2. Model Error / Uncertainty ($\Delta$)

* **robust control**에서 가장 중요한 오차

* 예를 들어, 이건 직접 측정되는 오차가 아니라, 제어 모델과 실제 시스템의 차이로서
    * Depth가 다름
    * Hand–Eye(카메라–EE) 오차
    * Jacobian이 실제와 다름
    * 로봇 관절 마찰/동역학 오차

* **robust control**은 $\Delta$를 직접 측정하지 않아도 $\Delta$가 존재해도 안정성을 유지하도록 설계하는 기법입니다.

#### 3. Disturbance Error (d): 외란

* 접촉 충격
* 이미지 노이즈
* 진동
* 불규칙한 힘

### 그렇다면, next step output으로 어