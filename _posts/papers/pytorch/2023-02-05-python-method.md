---
layout: post
title: class
category: Python
tag: method
---

### Variables for class

```python
class Calculator(object):
    class_variable = 0 # class variable

    def __init__(self):
        Calculator.class_variable = 1
```

#### class variable

    class를 정의할 때, `method` 밖에 선언되어 있는 variable로서, 해당 class를 선언한 모든 instance에 대해서 공통으로 사용되는 variable이며 `class명.variable명`으로 접근한다.

#### instance variable 

    정의된 class는 여러 instance로 선언되어 사용될 수 있고, 각 instance마다 독립적인 variable을 선언하여 사용할 수 있는데 이를 `instance variable`이라고 한다.


### Methods

#### 1. `instance method`

    instance에 지정되어 있는 변수에 접근할 수 있도록 해주는 것으로, `self`를 첫 번째 argument로 적고, 나머지 파라미터를 그 다으메 전달한다.

#### 2. `static method` 

    해당 class에 대한 instance를 선언하고 이 instance를 통해서 접근할 수 없는 method를 의미하고, `@staticmethod`의 decorator를 통해서 선언한다.

#### 3. `class method`

    `@classmethod`의 decorator를 통해서 선언하며 `self` 대신에 `cls`라는 argument를 이용하여 해당 class에 선언된 변수에 접근을 한다. 


일바적으로 class의 instance를 통해서 접근할 필요가 없는 경우 `class method` 또는 `static method`를 사용하고, 해당 function이 class에 정의된 변수에 접근할 필요가 있으면 `class method`를 사용하고, 그렇지 않으면 `static method`를 사용한다.


```python
class Calculator(object):
    class_variable = 0 # class variable

    def __init__(self):
        Calculator.class_variable = 1

    # class method
    def add(self, x, y): 
        return x + y

    @staticmethod
    def is_zero(a):
        return a == 0

    @classmethod
    def print_class_variable(cls):
        print(Calculator.class_variable)
```


