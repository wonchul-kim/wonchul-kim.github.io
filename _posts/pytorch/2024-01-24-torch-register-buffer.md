---
layout: post
title: nn.Module.register_buffer()
category: Pytorch
tag: torch.nn
---


### `nn.Module`을 `gpu/cuda`로 로드할 때, `nn.Parameter()`로 설정하지 않은 일반 `tensor` 및 변수를 제외하고는 로드되지 않는다. 
```python
class Model(nn.Module):
	def __init__(self):
		super().__init__()
		
		self.param = nn.Parameter(torch.randn([2, 2]))
		
		buff_1 = torch.randn([2, 2])
		self.register_buffer('buff_1', buff_1)

		buff_2 = torch.randn([2, 2], requires_grad=True)
		self.register_buffer('buff_2', buff_2)

		self.non_buff = torch.randn([2, 2])

	def forward(self, x):
		return x
```

```python

model = Model()
print(model.param.device)    # cpu
print(model.buff_1.device)     # cpu
print(model.buff_2.device)     # cpu
print(model.non_buff.device) # cpu

model.cuda()
print(model.param.device)    # cuda:0
print(model.buff_1.device)     # cuda:0
print(model.buff_2.device)     # cuda:0
print(model.non_buff.device) # cpu
```

`nn.Module` 내부의 변수끼리 연산을 할 경우, `device`가 서로 mismatch되는 것을 방지하기 위해서 사용이 가능할 거 같다.


### `requires_grad=True`해도 `nn.Parameter()`로 인식되지 않기 때문에 `optimizer`에 영향을 받지 않는다.

```python
for name, param in model.named_parameters():
	print(name, param.data)

# param tensor([[...]])
```
