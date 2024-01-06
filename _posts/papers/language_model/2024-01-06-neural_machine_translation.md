---
layout: post
title: Nueral machine translation
category: Language Model
tag: language  attention
---


# [NEURAL MACHINE TRANSLATION](https://arxiv.org/abs/1409.0473)

#### Problem: 
<img src='/assets/neural_machine_translation/rnn_problem.png'>

- fixed `context vector`로 input으로 들어온 문장을 함축시키기에는 한계가 존재

#### Solution: 
<img src='/assets/neural_machine_translation/rnn_solution.png'>

- fixed `context vector`를 사용하는 것이 아닌, `encoder`에서의 모든 `state`를 활용하여 dynamic `context vector`를 생성하도록 한다. 

- `self-attention`을 활용하여 `state`를 활용할 때, `weight`를 적용한다.


#### Attention

<img src='/assets/neural_machine_translation/start.png'>

<img src='/assets/neural_machine_translation/2nd.png'>

<img src='/assets/neural_machine_translation/3rd.png'>

<img src='/assets/neural_machine_translation/last.png'>

1. `context vector`는 `decoer`의 `state`에 따라서 달라진다. 

2. 이 때, `encoder`의 모든 `state`를 사용하되, `decoder`에서 생성하는 `state`에 따라 다른 `weight`를 적용한다.  


#### Teacher Forcing

`decoder`의 여러 `state` 중에서 이미 틀린 `state`가 예측되는 경우, 이로 인해 뒤의 모든 `state`에 영향이 미치므로, 강제로 정답을 넣어주어서 학습을 좀 더 효율적으로 하는 trick같은 방법이다. 

1. 틀린 예측을 함
<img src='/assets/neural_machine_translation/teacher_forcing_1.png'>

2. 이에 다음 `state`에 영향을 미침
<img src='/assets/neural_machine_translation/teacher_forcing_2.png'>

3. 정답을 넣어줌으로서 학습을 효율적으로 함
<img src='/assets/neural_machine_translation/teacher_forcing_3.png'>

## References:

- [NEURAL MACHINE TRANSLATION](https://arxiv.org/abs/1409.0473)

- [Google Colab](https://colab.research.google.com/github/tensorflow/tensorflow/blob/r1.9/tensorflow/contrib/eager/python/examples/nmt_with_attention/nmt_with_attention.ipynb)

- [Simple tutorial in youtube](https://www.youtube.com/watch?v=WsQLdu2JMgI&list=PLVNY1HnUlO26qqZznHVWAqjS1fWw0zqnT&index=13&ab_channel=MinsukHeo%ED%97%88%EB%AF%BC%EC%84%9D)