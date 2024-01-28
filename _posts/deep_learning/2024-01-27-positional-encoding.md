---
layout: post
title: Positional Encoding
category: Deep Learning
tag: [positional encoding]
---

**Transformer**에서 **positional encoding**은 병렬 연산으로 인해서 잃게되는 위치정보를 반영하기 위한 기법이다. 

1. 한 문장에서의 각각의 단어는 서로 다른 positional encoding이 적용되어야 한다.

2. 다른 문장이더라도 동일한 위치의 단어에 대해서는 동일한 positional encoding이 적용되어야 한다. 

3. word embedding에 대해서 concatenation이 아니라 summation을 수행하기 때문에 너무 큰 값을 가지게 되면 word의 정보를 잃게 되기 때문에 작아야 한다. 



$$
PE_{(pos, 2i)} = sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}}) 
$$

$$
PE_{(pos, 2i + 1)} = cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}})
$$

<img src='/assets/deep_learning/positional_encoding/pos_i_d.png'>

- $d$: 하나의 단어를 표현하는데 사용된 `embedding`의 크기 (논문에서는 512)
- $i$: $pos$에 해당하는 위치 벡터의 index
- $pos$: 단어의 index

벡터 내부의 원소 위치에 따라 다양한 진폭의 sin, cos 그래프가 그려지며, 개별 원소는 원소 내 위치(i)와 문장 내 단어의 위치(pos)의 위치에 따라 값을 부여받게 됩니다. 아래의 그림을 보면 i의 크기에 따라 그래프의 진폭이 다름을 확인할 수 있고 이를 통해서 최대한 중복되지 않은 위치 임베딩을 생성할 수 있게 됩니다. 그리고 이러한 방식으로 512개 embedding의 원소들이 sin과 cos 값으로 번갈아가며 채워지게 됩니다.


## References:
- https://yangoos57.github.io/blog/DeepLearning/paper/Transformer/positional-encoding/
- https://www.blossominkyung.com/deeplearning/transfomer-positional-encoding#81382b13-c0f1-47ba-8fb5-08473612669e
- https://gaussian37.github.io/dl-concept-positional_encoding/