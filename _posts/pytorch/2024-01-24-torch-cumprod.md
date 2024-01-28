---
layout: post
title: torch.cumprod
category: Pytorch
tag: torch.cumprod
---

```
torch.cumprod(input, dim, *, dtype=None, out=None) -> Tensor
```

```python

import torch

# Create a 1D tensor of size 5
y = torch.tensor([1, 2, 3, 4, 5])

# Calculate the cumulative product of y along dimension 0
c = torch.cumprod(y, dim=0)
print(c)  
# tensor([  1,   2,   6,  24, 120])

# Create a 2D tensor of size 2x3
z = torch.tensor([[1, -2, -3],
                  [4, -5, -6]])

# Calculate the cumulative product of z along dimension 1
c_row = torch.cumprod(z, dim=1)
print(c_row)  
# tensor([[  1,  -2,   6],
#         [  4, -20, 120]])
```