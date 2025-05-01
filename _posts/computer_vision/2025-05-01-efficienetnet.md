---
layout: post
title: EfficientNet
category: Computer Vision
tag: [efficientNet]
---

#### `tf.keras.tf.keras.applications.EfficientNetBX` (X = [0, 1, 2, 3, 4, 5, 6, 7])


0 =
<KerasTensor: shape=(None, 560, 384, 144) dtype=float32 (created by layer 'block2a_expand_activation')>
1 =
<KerasTensor: shape=(None, 280, 192, 192) dtype=float32 (created by layer 'block3a_expand_activation')>
2 =
<KerasTensor: shape=(None, 140, 96, 288) dtype=float32 (created by layer 'block4a_expand_activation')>
3 =
<KerasTensor: shape=(None, 70, 48, 576) dtype=float32 (created by layer 'block5a_expand_activation')>
4 =
<KerasTensor: shape=(None, 70, 48, 816) dtype=float32 (created by layer 'block6a_expand_activation')>
5 =
<KerasTensor: shape=(None, 35, 24, 1392) dtype=float32 (created by layer 'block7a_expand_activation')>
6 =
<KerasTensor: shape=(None, 35, 24, 1536) dtype=float32 (created by layer 'top_activation')>


When input shape is (512, 512, 3) as (w, h, ch) and tf uses (bs, w, h, ch):

B0, 1

- `block2a_expand_activation`: (bs, 256, 256, 96)
- `block3a_expand_activation`: (bs, 128, 128, 144)
- `block4a_expand_activation`: (bs, 64, 64, 240)
- `block6a_expand_activation`: (bs, 32, 32, 672)



B2

- `block2a_expand_activation`: (bs, 256, 256, 96)
- `block3a_expand_activation`: (bs, 128, 128, 144)
- `block4a_expand_activation`: (bs, 64, 64, 288)
- `block6a_expand_activation`: (bs, 32, 32, 720)


B3

- `block2a_expand_activation`: (bs, 256, 256, 144)
- `block3a_expand_activation`: (bs, 128, 128, 192)
- `block4a_expand_activation`: (bs, 64, 64, 288)
- `block6a_expand_activation`: (bs, 32, 32, 816)


#### `tf.keras.tf.keras.applications.EfficientNetV2BX` (X = [0, 1, 2, 3, 4, 5, 6, 7])

B0
- 2: [None, 128, 128, 64]
- 3: [None, 64, 64, 128]
- 4: [None, 64, 64, 192]
- 5: [None, 32, 32, 576]
- 6: [None, 32, 32, 672]
- top: [None, 16, 16, 1280]

B1
[None, 128, 128, 64]
[None, 64, 64, 128]
[None, 64, 64, 192]
[None, 32, 32, 576]
[None, 32, 32, 672]
[None, 16, 16, 1280]

B2
[None, 128, 128, 64]
[None, 64, 64, 128]
[None, 64, 64, 224]
[None, 32, 32, 624]
[None, 32, 32, 720]
[None, 16, 16, 1408]

B3
- 1: [None, 128, 128, 64]
- 2: [None, 64, 64, 160]
- 3: [None, 64, 64, 224]
- 4: [None, 32, 32, 672]
- 5: [None, 32, 32, 816]
- post: [None, 16, 16, 1536]

S
[None, 128, 128, 96]
[None, 64, 64, 192]
[None, 64, 64, 256]
[None, 32, 32, 768]
[None, 32, 32, 960]
[None, 16, 16, 1280]