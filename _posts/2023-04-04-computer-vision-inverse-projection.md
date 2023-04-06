---
layout: post
title: Inverse Projection Transformation
category: Computer Vision
tag: inverse projection
---

- Projective transformation: 눈으로 보이는 3D의 실제 영상이 사진으로 찍었을 때, 사진상으로는 2D로 바뀌면서 depth information을 잃는 transformation.

- Inverse Projective transformation: 2D의 이미지 정보를 바탕으로 3D로 transformation하는 것으로, depth정보가 필요하다.

- [perspective projection](https://towardsdatascience.com/depth-estimation-1-basics-and-intuition-86f2c9538cd1)

To perform the 3D reconstruction, we will go through camera calibration parameters, projective transformation using intrinsic and its inverse, coordinate transformation between frames.

### Central projection of pinhole camera model

눈으로 보이는 3D의 실제 영상이 사진으로 찍혀서 2D가 되는 수식을 다음과 같이 표현하면,

$$(u, v) = f(X, Y, Z)$$
$$sp = K[R|t]*P$$
$$s\begin{bmatrix}u\\v\\1\\ \end{bmatrix} = \begin{bmatrix}f_x&0&u_0\\0&f_y&v_0\\0&0&1\\ \end{bmatrix}\begin{bmatrix}r_{11}&r_{12}&r_{13}&t_1\\r_{21}&r_{22}&r_{23}&t_2\\r_{31}&r_{32}&r_{33}&t_3\\ \end{bmatrix}\begin{bmatrix}X\\Y\\Z\\1\\ \end{bmatrix}$$
$$ eq. 1 $$

- $p$: projected point on the image plane
- $K$: camera instinsics matrix
- $[R|t]$: camera frame와 world frame간의 변환을 위한 extrinsics parameters
- $[X, Y, Z]$: world coordinate 기준 3D 포인트(Euclidean space)
- $s$: focal length가 변화함에 따른 $x, y$의 pixel변화를 나타내는 aspect ratio

#### Intrinsic parameter matrix

- focal length ($f_x, f_y$): measure the position of the image plane wrt to the camera centre.

- principal point ($u_0, v_0$): optical centre of the image plane

- skew factor: misalignment from a square pixel if the image plane axes are not perpendicular.

- checkerboard method

- means of PnP, Idrect Linear Transform, or RANSAC.

- Consider the equation in eq.1, suppose (X, Y, Z, 1) is in the camera coordinate frame. i.e. we do not need to consider the extrinsic matrix $[R|t]$. Expanding the equation would give as

$$u = \frac{1}{s_x}f\frac{X}{Z} + o_x$$

$$v = \frac{1}{s_y}f\frac{X}{Z} + o_y$$

The 3D points can be recovered with Z given by the depth map and solving for X and Y. We can then further transform the points back to the world frame if needed.

### references

- [Inverse Projection Transformation](https://towardsdatascience.com/inverse-projection-transformation-c866ccedef1c)
