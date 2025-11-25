---
layout: post
title: Orientation
category: Robots
tag: [orientation]
---



# Orientation


## Quaternion

$$q = (vx * sin(θ/2), vy * sin(θ/2), vz * sin(θ/2), cos(θ/2))$$

* $(vx, vy, vz)$는 회전축(단위 벡터), $θ$는 회전각

* `단위 쿼터니언`이어야 함

* $-q = q$

    * 회전축이 반대로 되는 만큼 회전각도도 반대로 표현되기 때문ㅇ

* singularity (euler의 gimbal lock)이 없음


#### Rotation matrix to Quaternion 
$$
q = (q_x, q_y, q_z, q_w)
$$

$$
\|q\| = \sqrt{q_x^2 + q_y^2 + q_z^2 + q_w^2}
$$

$$
\tilde{q}_x = \frac{q_x}{\|q\|},\quad
\tilde{q}_y = \frac{q_y}{\|q\|},\quad
\tilde{q}_z = \frac{q_z}{\|q\|},\quad
\tilde{q}_w = \frac{q_w}{\|q\|}
$$

#### Quaternion to Rotation matrix

$$
R =
\begin{bmatrix}
1 - 2(q_y^2 + q_z^2) &
2(q_x q_y - q_z q_w) &
2(q_x q_z + q_y q_w)
\\[4pt]
2(q_x q_y + q_z q_w) &
1 - 2(q_x^2 + q_z^2) &
2(q_y q_z - q_x q_w)
\\[4pt]
2(q_x q_z - q_y q_w) &
2(q_y q_z + q_x q_w) &
1 - 2(q_x^2 + q_y^2)
\end{bmatrix}
$$


## Euler

* (roll, pitch, yaw)

* singularity (euler의 gimbal lock) 이슈 존재


#### Euler to Rotation matrix

$$
\text{roll} = \phi,\quad
\text{pitch} = \theta,\quad
\text{yaw} = \psi
$$

$$
R = R_z(\psi) R_y(\theta) R_x(\phi)
$$

$$
R_x(\phi) =
\begin{bmatrix}
1 & 0 & 0\\
0 & \cos\phi & -\sin\phi\\
0 & \sin\phi & \cos\phi
\end{bmatrix},\quad
R_y(\theta) =
\begin{bmatrix}
\cos\theta & 0 & \sin\theta\\
0 & 1 & 0\\
-\sin\theta & 0 & \cos\theta
\end{bmatrix},\quad
R_z(\psi) =
\begin{bmatrix}
\cos\psi & -\sin\psi & 0\\
\sin\psi & \cos\psi & 0\\
0 & 0 & 1
\end{bmatrix}
$$

$$
R =
\begin{bmatrix}
c_\psi c_\theta &
c_\psi s_\theta s_\phi - s_\psi c_\phi &
c_\psi s_\theta c_\phi + s_\psi s_\phi
\\
s_\psi c_\theta &
s_\psi s_\theta s_\phi + c_\psi c_\phi &
s_\psi s_\theta c_\phi - c_\psi s_\phi
\\
-s_\theta &
c_\theta s_\phi &
c_\theta c_\phi
\end{bmatrix}
$$

$$
c_\psi = \cos\psi,\quad s_\psi = \sin\psi,\quad
c_\theta = \cos\theta,\quad s_\theta = \sin\theta,\quad
c_\phi = \cos\phi,\quad s_\phi = \sin\phi
$$


#### Rotation matrix to Euler
$$ R = [r_{ij}] (i행 j열 원소)$$

$$
R =
\begin{bmatrix}
r_{11} & r_{12} & r_{13}\\
r_{21} & r_{22} & r_{23}\\
r_{31} & r_{32} & r_{33}
\end{bmatrix}
$$

$$
\theta = \text{pitch} = \arcsin(-r_{31})
$$

$$
\phi = \text{roll} = \arctan2(r_{32},\, r_{33})
$$

$$
\psi = \text{yaw} = \arctan2(r_{21},\, r_{11})
$$





