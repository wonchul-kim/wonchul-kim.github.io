---
layout: post
title: torch.gather
category: Pytorch
tag: torch.nn
---

```python
torch.gather(input, dim, index, out=None, sparse_grad=False)
```

해당 `input`에 대한 `index`의 value들을 반환한다. 

- `input`과 `index`는 `dim`의 값에 대한 차원을 제외하고는 동일한 shape이어야 한다. (e.g. input: (10, 10, 10), index: (10, 1, 10), dim=1)
- 즉, `dim`에 해당하는 차원에 대한 `index`를 바탕으로 value를 반환한다. 

