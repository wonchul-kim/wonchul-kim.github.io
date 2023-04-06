---
layout: post
title: Homogeneous coordinate
category: Computer Vision
tag: homogeneous coordinate
---

- `Affine transformation`: 선형변환 + 선형이동

- `vector`만 존재하는 `vector space`에서 `point`의 개념을 사용하여 위치 정보가 추가된 공간을 `affine space`라고 한다.

- `vector space`에서의 `transformation`은 `linear transformation`이고, `affine space`에서의 `transformation`은 `affine transformation`이라고 한다.

- `vector space`와 `affine sapce`의 가장 큰 차이점은 이동성분:

$$y = Ax \tag{ep.1. linear tranformation}$$
$$y = Ax + b \tag{eq.2 affine transformation}$$

이 때, `affine tranformation`을 eq.1처럼 단순히 transformation matrix의 곱으로 표현하려면 `homogeneous coordinate`가 필요하다.

$$u = \begin{bmatrix}u_x\\u_y\\u_z\\0 \end{bmatrix} \tag{eq.3}$$
$$v = \begin{bmatrix}v_x\\v_y\\v_z\\1 \end{bmatrix} \tag{eq.4}$$

위와 같이 `vector`의 transformation과 `point`의 transformation을 하나의 matrix에 표현하기 위해서 eq.3에 하나의 차원을 더 추가한 eq.4를 사용하여, `vector`와 `point`에 대한 transformation matrix를 한 번에 표현한다.

`homogeneous coordinate`를 사용하는 이유는 `projection`과 관련하여 3D에서 2D로 투영하는 작업을 하기 위해서이고, 또는 3D의 `affine transformation`을 모두 통일된 표현으로 하여금 수식을 편의화하기 위해서이다.
