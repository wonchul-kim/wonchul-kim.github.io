---
layout: post
title: Albumentations - TpTensor
category: Augmentation
tag: albumentations, totensor
---

## PILToTensor (torchvision) vs. ToTensor (albumentations.pytorch)

1. PILToTensor

```python
    transforms = [
            (... some augmentation technicques)
            T.PILToTensor(),
            T.ConvertImageDtype(torch.float),
            T.Normalize(mean=mean, std=std),
    ]
```

`PILToTensor` transforms a `PIL.Image` to a `PyTorch tensor` of the same dtype

#### segmentation

위의 과정을 진행하면, image는 `torch.float32`이고, target/mask는 `torch.int64`의 dtype이 된다.

> target/mask를 `torch.float32`로 변경하여 학습하면, 학습이 안된다.

2. ToTensor

`ToTensor` normalizes the tensor to [0, 1] and will return a `FloatTensor`(torch.float64), but this is considered as `torch.cuda.DoubleTensor` when the data is passed to cuda.

Thus, this might cause the below problem:

```
Input type (torch.cuda.DoubleTensor) and weight type (torch.cuda.FloatTensor) should be the same
```

Then, before putting the `data` into model, the below line of code can solve the problem:

```
data.to(device=device, dtype=torch.float32).
```

#### segmentation

`ToTensor()`를 사용한다면, 아래를 진행해서 꼭 데이터 타입을 맞추자

```
image, target = image.to(device=device, dtype=torch.float32), target.to(device=device, dtype=torch.int64)
```

## Usually normalized/standardized tensors are preferred while training a model as the convergence often benefits from it.
