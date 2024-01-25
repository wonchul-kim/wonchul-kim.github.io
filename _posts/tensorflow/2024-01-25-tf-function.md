---
layout: post
title: tf.function()
category: Tensorflow
tag: tf.function
---

Tensorflow 2.x에서는 직관적인 `Eager Execution`이 default로 설정되어 있지만, 배포와 성능 측면에서는 비효율적이다. 그렇기 때문에, `tf.function()`로 정의된 함수를 미리 `Graph`로 정의하여 최적화하여 배포와 성능 측면에서 효율적으로 사용할 수 있다. 예를 들어, `tf.funciton()`을 사용하면 동일한 데이터셋/파라미터의 조건에서 GPU 메모리를 더 적게 쓰면서 학습이 가능하다. 

> 일반적으로 `Graph`가 `Eager`보다 빠르며, 특히 작은 연산을 여러 번 수행하는 경우에 `Graph` 모드가 유리하다. 하지만, `Convolution`처럼 큰 연산을 적은 횟수로 하는 경우에는 속도 향상이 미미하거나 오히려 느려질 때도 있다. 


### Tracing

- `Grpah`는 ConcreteFunction으로 래핑된다.
- `tf.function()`은 캐시되어있는 ConcreteFunction을 관리하고, 입력에 맞는걸 가져온다.
- `tf.function()`은 Python function을 래핑하고 Function Object를 리턴한다.
- `Tracing`은 Graph를 만들고, ConcreteFunction으로 래핑하는 동작을 말한다.
- `Tracing`을 줄일수록 TF의 Performance를 최적화 할 수 있다.

### Debugging

- `@tf.function`을 사용하지 않고, `eager`모드로 먼저 debugging을 진행해서 문제가 없는지 확인한다. 또는 `tf.config.run_functions_eagerly(True)`를 선언하면 `@tf.function`이 비활성화된다. 

- `graph`모드에서는 `Tensor`를 `numpy`로 바꿀 수가 없다. 

- `graph`모드에서는 break point를 걸어서 디버깅이 불가능하다.

- `print()`는 `tracing` 동작에서만 수행된다. 그렇기 때문에 `Trace`를 추적할 때 사용하기 용이하다.

- `pdb()`도 `tracing` 동작에서 추적하기 용이하지만, `AutoGraph`로 변환이 이루어지면 사용 불가능하다.

- `tf.print()`는 항상 동작한다. 그렇기 때문에 실제 함수내에서의 value를 추적할 때 용이하다. 

- `tf.debugging.enable_check_numeric`는 `NaN`이나 `Inf`가 만들어지는 시점을 추적하기 용이하다. 


### References:
- [Better performance with tf.function](https://www.tensorflow.org/guide/function?hl=en)
- https://jhtobigs.oopy.io/cb9020dc-51b6-4146-b398-3d1704264d1d