---
layout: post
title: FoundationPose
category: 3D Computer Vision
tag: [foundationpose]
---

# [FoundationPose](https://arxiv.org/pdf/2312.08344)


* Unified framework

  * pose estimation and tracking for novel objects in both

    * model-based: CAD 정보를 사용하여 추론

    * model-free: CAD 정보가 없다면, 최대 16장의 여러 view에서의 이미지를 토대로 mesh를 생성하여 추론

* Strong generalization

  * large-scale synthetic training by large language model

  * transformer-based architecture 

  * constrative learning formulation

#### Neural Object Modeling

* **NeRF**계열의 neural 아이디어를 빌리긴 하지만, **SDF**기반(NeuS류) 표면 `모델+고속 렌더링`으로 포즈를 푸는 방식
  * 그래서 **NeRF** 그 자체는 아니다

*-* SDF 기반의 Neural Object Field를 참조 이미지(소수, 보통 ≤16장)로 한 번만 만들어 놓고, 이후엔 이 필드를 이용해 다양한 자세에서 RGB-D를 고속 렌더링 → render-and-compare로 포즈를 정제·선택
* 그래서 model-based와 동일한 다운스트림(정제·스코어링) 모듈을 그대로 쓰게끔 “갭”을 메운다.
* 기존 few-shot 계열(OnePose/FS6D 등)은 2D–3D 대응점에 많이 의존해서 무질감/가림에서 부서지기 쉬움. FoundationPose는 대응점 매칭 없이 render-and-compare로 밀어붙이기 때문에, CAD 부재를 고품질 뉴럴 렌더러로 대체해야 함

* 소수의 이미지 (~16장) & 카메라 포즈
  * SDF (geometry)와 색상 (appearance) 를 학습
  
  * Field Representation
      * `Compared to NeRF [44], the SDF representation Ω provides higher
      quality depth rendering while removing the need to manually select a density threshold.`
  * Field Learning


* `cam_in_obs`: 월드 좌표계를 기준으로 한 카메라의 포즈
$$T_{world \larr camera}$$

* `tcp_pos`: $$T_{base \leftarrow tcp}$$