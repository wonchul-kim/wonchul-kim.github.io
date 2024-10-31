---
layout: post
title: Self-attention basics
category: Computer Vision
tag: [self-attention]
---

#### Image Denosing

노이즈를 제거 하기 위한 여러 가지의 filter(Box Filter, Gaussian Filter, Median Filter, ...)들이 있으며, 이들은 "노이즈를 제거하고자 하는 픽셀이 주변의 픽셀과 유사할 것"이라는 가정하에 진행된다. 
하지만, 이런 가정은 **Edge**에서는 성립하지 않는다. 

이를 극복하기 위해서 **Local Filter**가 고안되었고, 그 중 **Bilateral Filter**은 다음과 같다.
$$
BF[I]_p = \frac{1}{W_p}\sum_{q \in S} G(||p - q||)G_{\sigma_{\gamma}}(|I_p - I_q|)I_q
$$

* 주변에 있는 값들만 참고해서 노이즈를 제거

* 이는 거리에 따른 Gaussian값과 intensity에 따른 Gaussian값을 곱해서 얻는다. 





