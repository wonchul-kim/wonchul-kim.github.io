---
layout: post
title: Stable Diffusion Complements DINO for Zero-Shot Semantic Correspondence
category: Computer Vision
tag: [zero-shot]
---

# [A Tale of Two Features: Stable Diffusion Complements DINO for Zero-Shot Semantic Correspondence](https://proceedings.neurips.cc/paper_files/paper/2023/file/8e9bdc23f169a05ea9b72ccef4574551-Paper-Conference.pdf)

## Abs.

*  In this work, we exploit Stable Diffusion (SD) features for semantic and dense correspondence and discover that with simple postprocessing, SD features can perform quantitatively similar to SOTA representations.

*  SD features have very different properties compared to existing representation learning features, such as the recently released DINOv2: while DINOv2 provides sparse but accurate matches, SD features provide high-quality spatial information but sometimes inaccurate semantic matches.

> a zero-shot evaluation via nearest neighbor search on the fused features provides a significant performance gain over state-of-the-art methods on benchmark datasets, e.g., SPair-71k, PF-Pascal, and TSS.


## Intro.

* text-to-image diffusion model은 text prompt로부터 이미지 자체에 영향을 미치는 것이기 때문에 이미지를 이해(task에 따라 sparse/dense prediction에 의해 이해의 의미/범주가 달라질듯)해야하고, diffusion model이 특정 scene을 생성하는 등 이미지에 영향을 미칠 수 있는 제어 요소가 있으므로 객체를 탐지하는 것도 가능할 것이다.

    * 즉, Diffusion model도 이미지를 이해하는 능력이 있다. 그러므로 객체 탐지 task에 도움이 될 것이다.

        * 하지만, 일반적으로 SD feature는 공간 정보가 우수하지만, 의미론적 정보가 부족한 경우가 있음

        * DINOv2 feature는 일부 지점에서 정확한 매칭을 제공하지만, 대응이 드물고 부분적임 -> 높은 의미론적 정보 & 드문 정확 매칭

            * DINOv2는 이미지 전체에서 “매우 의미적으로 잘 맞는 지점”을 소수만 강하게 매칭함. 
            * 예를 들어, 두 동물 이미지에서 “귀 끝”, “코 끝” 등 명확한 의미를 가진 특징적인 위치들만 정확히 잘 찾아내고, 전반적인 픽셀마다 일치시키려면 정보가 부족
            * 쉽게 말해, “중요한 곳에는 정확히 대응을 잡아주지만, 대응 자체가 많지는 않다”는 뜻입니다.

            * DINOv2는 특히 고수준 의미 정보에 강하며, 작은 패치의 모양이나 위치에서 의미적 일치가 강하게 잡히는 곳에서만 좋은 답을 내어놓게 설계된 특성이 있ㄹㄹ,ㄴ


* 하지만, 대부분 single image에 대해서 연구가 되었고, 이는 multiple, different images and objects에 연관된 feature에 대해서 아는 것이 거의 없기 때문이다. 

* 본 논문은 **focus on understanding how features in different images relate to one another by examining Stable Diffusion features through the lens of semantic correspondences, aiming to connect similar pixels in two or more images.**


> **Correspondence Task**: Computer vision 에서 두 이미지간에 동일하거나 유사한 부분(point, pixel, patch, object, ...)을 찾는 문제 
>   * 스테레오 매칭: 한 장면을 다른 각도에서 촬영한 두 이미지 사이의 동일 위치 찾기(3D 재구성 등)
>   * 옵티컬 플로우: 시간적으로 인접한 영상에서 같은 객체의 이동 추적
>   * 세맨틱 대응(semantic correspondence): 다른 인스턴스이지만, 같은 클래스(예: 두 마리 코끼리) 이미지 간에 같은 부위(귀, 다리 등) 매칭
>   * 이미지 스티칭 및 파노라마: 서로 중첩되는 이미지를 하나로 합칠 때 동일 포인트 탐지 


## References
- [A Tale of Two Features: Stable Diffusion Complements DINO for Zero-Shot Semantic Correspondence](https://proceedings.neurips.cc/paper_files/paper/2023/file/8e9bdc23f169a05ea9b72ccef4574551-Paper-Conference.pdf) [code](https://github.com/Junyi42/sd-dino)