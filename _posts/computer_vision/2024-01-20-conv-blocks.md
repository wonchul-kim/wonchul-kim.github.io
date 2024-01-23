---
layout: post
title: Convolution Blocks
category: Computer Vision
tag: conv blocks
---

### ConvNormActBlock

```python
class ConvNormActBlock(nn.Sequential):
    def __init__(self, in_channels, out_channels, kernel_size, norm=nn.BatchNorm2d, act=nn.ReLU, **kwargs):
        super().__init__(
            nn.Conv2d(in_channels, out_channels, kernel_size=kernel_size, padding=kernel_size//2),
            norm(out_channels),
            act(),
        )        

Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
x = torch.randn((1, 32, 224, 224))
Conv1x1BnReLU(32, 32)(x).shape
```

> torch.Size([1, 32, 224, 224])


### ResidualAddBlock

<img src='/assets/computer_vision/conv_blocks/residual.png'>

이 때, `projection shorcut`은 입력의 차원과 출력의 차원이 다를 때 사용한다. 

```python
class ResidualAddBlock(nn.Module):
    """
        https://arxiv.org/abs/1512.03385
    """
    def __init__(self, block, shorcut=None):
        super().__init__()
        self.block = block
        self.shorcut = shorcut

    def forward(self, x):
        _x = x
        x = self.block(x)
        if self.shorcut:
            _x = self.shorcut(_x)
        x += _x
        return x

x = torch.randn((1, 32, 224, 224))
print(ResidualAddBlock(nn.Conv2d(32, 32, kernel_size=1))(x).shape)
print(ResidualAddBlock(nn.Conv2d(32, 64, kernel_size=1), shorcut=nn.Conv2d(32, 64, kernel_size=1))(x).shape)
```

> torch.Size([1, 32, 224, 224]) <br>
torch.Size([1, 64, 224, 224])


### BottleNeckBlock

<img src='/assets/computer_vision/conv_blocks/bottleneck.png'>


모델에서 `CNN`의 연산량을 줄이기 위해서 pooling을 주로 사용하며, 이는 feature의 `width`/`height`의 차원을 줄인다. **BottleNeck**도 마찬가지로 연산량을 줄이기 위한 목적으로 `channel`의 차원을 축소한다. 

이 때, `1x1 Convolution[PointWise Block]`을 사용하고, `kernel size`가 1이기 때문에 spatial 정보를 갖고 있지 않으며 오직 연산량을 줄이기 위한 목적으로 사용된다. 

<img src='/assets/computer_vision/conv_blocks/bottleneck_plot.png'>

결국에는 연산량과 정보손실은 서로 trade-off의 관계이지만, 정보손실이 없는 정도로 연산량을 줄일 수 있는 구간을 찾자는 것이 목적이다. 

```python
class BottleNeckBlock(nn.Sequential):
    """
        https://arxiv.org/abs/1512.03385
    """
    def __init__(self, in_channels, out_channels, reduction):
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                ResidualAddBlock(
                    nn.Sequential(
                        Conv1x1BnReLU(in_channels, out_channels//reduction),
                        Conv3x3BnReLU(out_channels//reduction, out_channels//reduction), 
                        Conv1x1BnReLU(out_channels//reduction, out_channels, act=nn.Identity)
                    ),
                    shorcut=Conv1x1BnReLU(in_channels, out_channels) if in_channels != out_channels else None
                ),
                nn.ReLU()
            )
        )

x = torch.randn((1, 32, 224, 224))
print(BottleNeckBlock(32, 32, 4)(x).shape)
```

> torch.Size([1, 32, 224, 224])

### LinearBottleNeckBlock

**BottleNeckBlock**과 동일하지만, 마지막에 `ReLU` activation이 없는 구조이다. 
`ReLU`를 사용하면, 0보다 작은 구간에서는 0으로 만들어버리기 때문에 어쩔 수 없이 정보손실이 일어날 수 밖에 없으며, 이는 채널의 수가 많은 경우에는 어느 정도 손실을 보완할 수 있다고 한다. 하지만, **BottleNeckBlock**은 채널의 수가 작가 때문에 오히려 정보의 손실만 일어난다는 하기 때문에 제거하여 그대로 도출하는 것이 **LinearBottleNeckBlock**이다. 

```python
class LinearBottleNeckBlock(nn.Sequential):
    """
        https://arxiv.org/abs/1801.04381
    """
    def __init__(self, in_channels, out_channels, reduction):
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                ResidualAddBlock(
                    nn.Sequential(
                        Conv1x1BnReLU(in_channels, out_channels//reduction),
                        Conv3x3BnReLU(out_channels//reduction, out_channels//reduction),
                        Conv1x1BnReLU(out_channels//reduction, out_channels, act=nn.Identity)
                    ),
                    shorcut=Conv1x1BnReLU(in_channels, out_channels) if in_channels != out_channels else None
                )
            )
        )

x = torch.randn((1, 32, 224, 224))
print(LinearBottleNeckBlock(32, 32, 4)(x).shape)
```

> torch.Size([1, 32, 224, 224])

### InvertedResidualBlock

```python
class InvertedResidualBlock(nn.Sequential):
    """
        https://arxiv.org/abs/1801.04381
    """
    def __init__(self, in_channels, out_channels, expansion):
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                ResidualAddBlock(
                    nn.Sequential(
                        Conv1x1BnReLU(in_channels, out_channels*expansion),
                        Conv3x3BnReLU(out_channels*expansion, out_channels*expansion),
                        Conv1x1BnReLU(out_channels*expansion, out_channels, act=nn.Identity)
                    ),
                    shorcut=Conv1x1BnReLU(in_channels, out_channels) if in_channels != out_channels else None
                ),
                nn.ReLU()
            )
        )

x = torch.randn((1, 32, 224, 224))
print(InvertedResidualBlock(32, 64, 4)(x).shape)
```

> torch.Size([1, 64, 224, 224])

#### **MobileNetV2**에서는 `Residual` 블락이 입력과 출력의 차원이 같은 경우에만 사용된다

```python

class InvertedResidualBlock(nn.Sequential):
    """
        https://arxiv.org/abs/1801.04381
    """
    def __init__(self, in_channels, out_channels, expansion):
        residual = ResidualAddBlock if in_channels == out_channels else nn.Sequential
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                residual(
                    nn.Sequential(
                        Conv1x1BnReLU(in_channels, out_channels*expansion),
                        Conv3x3BnReLU(out_channels*expansion, out_channels*expansion),
                        Conv1x1BnReLU(out_channels*expansion, out_channels, act=nn.Identity)
                    ),
                ),
                nn.ReLU()
            )
        )

x = torch.randn((1, 32, 224, 224))
print(InvertedResidualBlock(32, 32, 4)(x).shape)
print(InvertedResidualBlock(32, 64, 4)(x).shape)
```

> torch.Size([1, 32, 224, 224]) <br>
torch.Size([1, 64, 224, 224])

### Depth-Wise Separable Convolution

`3x3 Convolution`의 연산량을 줄이기 위해서 `3x3 Convolution`을 두 가지의 연산 수행으로 나눈 것이다. 먼저, 하나의 `3x3 Convolution`으로 input의 각 채널마다 연산을 수행하고, `1x1 Convolution`으로 모든 채널에 대해서 연산을 수행한다. 이는 일반적인 `3x3 Convolution`과 동일하지만, 파라미터를 매우 많이 줄일 수 있다. 

```python
class DepthWiseSeparableConv(nn.Sequential):
    def __init__(self, in_channels, out_channels):
        super().__init__(
            nn.Conv2d(in_channels, in_channels, kernel_size=3, groups=in_channels),
            nn.Conv2d(in_channels, out_channels, kernel_size=1)
        )

x = torch.randn((1, 32, 224, 224))
print(DepthWiseSeparableConv(32, 64)(x).shape)
```

> torch.Size([1, 64, 222, 222])

```python
sum(p.numel() for p in DepthWiseSeparableConv(32, 64).parameters() if p.requires_grad)
# 2432
sum(p.numel() for p in nn.Conv2d(32, 64, kernel_size=3).parameters() if p.requires_grad)
# 18496
```

### MBConv

```python
class MBConv(nn.Sequential):
    def __init__(self, in_channels, out_channels, expansion):
        residual = ResidualAdd if in_channels == out_channels else nn.Sequential
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                residual(
                    nn.Sequential(
                        Conv1x1BnReLU(in_channels, in_channels*expansion, act=nn.ReLU6),
                        Conv3x3BnReLU(in_channels*expansion, in_channels*expansion, groups=in_channels*expansion, act=nn.ReLU6),
                        # SE Block
                        Conv1x1BnReLU(in_channels*expansion, out_channels, act=nn.Identity),
                    ),
                ),
                nn.ReLU(),
            )
        )

x = torch.randn((1, 32, 224, 224))
print(MBConv(32, 64, 4)(x).shape)
```

> torch.Size([1, 64, 224, 224])

### Fused Inverted Residual (Fused MBConv)

```python
class FusedMBConv(nn.Sequential):
    """
        https://arxiv.org/abs/2104.00298
    """
    def __init__(self, in_channels, out_channels, expansion):
        residual = ResidualAdd if in_channels == out_channels else nn.Sequential
        Conv1x1BnReLU = partial(ConvNormActBlock, kernel_size=1)
        Conv3x3BnReLU = partial(ConvNormActBlock, kernel_size=3)
        super().__init__(
            nn.Sequential(
                residual(
                    nn.Sequential(
                        Conv3x3BnReLU(in_channels, in_channels*expansion, act=nn.ReLU6),
                        # SE Block
                        Conv1x1BnReLU(in_channels*expansion, out_channels, act=nn.Identity),
                    ),
                ),
                nn.ReLU(),
            )
        )

x = torch.randn((1, 32, 224, 224))
print(FusedMBConv(32, 64, 4)(x).shape)
```

> torch.Size([1, 64, 224, 224])
