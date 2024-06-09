---
layout: post
title: 3D Gaussian Splatting
category: 3D Computer Vision
tag: gaussian splatting
---

[3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://arxiv.org/abs/2308.04079)

- Quality, training time, rendering speed 에서 각 분야의 SOTA보다 더 좋아서 이슈가 됨
- trade-off 관계인데도 불구하고 모든 성능에서 더 좋은 수치를 보여줌

- 3D 공간상에 수 많은 3D Gaussian이 모여서 하나의 scene을 구성

- Why 3D Gaussian?
    - differentiable volumetric representation
    - explicit representation
    - effective for 3D to 2D projection and $\alpha$-blending in rasterization
        - $\alpha$-blending: 투명도 값의 더하기 연산
    - fast rendering

    - Comparsion to NeRF: 
        - NeRF: 
            1. 이미지 각 pixel마다 ray를 그려서 여러 점을 샘플링
            2. 샘플링한 각 점의 color와 volume density를 계산
            3. ray위의 이 값들을 summation 하여 이미지를 Rendering
        - 3D Gaussian Splatting:
            1. 이미지를 14x14 pixel로 구성된 Tile들로 나누고
            2. Tile마다 Gaussian들을 Depth로 정렬
            3. 앞에서 부터 뒤로 순차적으로 Alpha blending 하여 이미지를 Rendering

            

<img src='/assets/3d_computer_vision/nerf/nerf_1.png'>



## References
- [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://arxiv.org/abs/2308.04079)

- [official code](https://github.com/graphdeco-inria/gaussian-splatting)