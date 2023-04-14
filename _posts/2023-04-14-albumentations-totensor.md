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

#### Usually normalized/standardized tensors are preferred while training a model as the convergence often benefits from it.
