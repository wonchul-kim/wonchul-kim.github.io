---
layout: post
title: singleton
category: Python
tag: singleton
---



### Singleton

singleton을 사용하는 이유는 해당 class에 대해서 하나의 instance만 생성하여 해당 instance에 대한 변수들을 공유하기 위해서이다. 

아래의 예시들을 보면, 해당 class에 대한 두 개의 instance에 대해서 변수가 공유되는 것을 알 수 있다. 

#### 1. `__new__()` 사용

python에서는 class를 instance로 생성할 때, `__new__()`가 호출되는데, 이 때 두 번째 instance가 생성되지 않도록 한다. 

```python

class SingletonClass():
    def __new__(cls):
        if not hasattr(cls, 'instance'):
            cls.instance = super(SingletonClass, cls).__new__(cls)
        return cls.instance

singleton1 = SingletonClass()
singleton1.my_variable = "my variable"
print(singleton1)
print(singleton1.my_variable)

singleton2 = SingletonClass()
print(singleton2)
print(singleton2.my_variable)
```
```python
<__main__.SingletonClass object at 0x7f3de899bca0>
my variable
<__main__.SingletonClass object at 0x7f3de899bca0>
my variable
```
#### 2. `@classmethod` 사용

python의 built-in function인 `__new__()` 대신에  `@classmethod`와 `class variable`을 사용하여 instance를 선언하고 공유할 수 있도록 한다. 그리고 해당 class 자체를 선언하여 instance를 생성하려고 하면 `__init__()`을 통해서 Error가 나도록 하여 완성도를 높인다.  

```python
class SingletonClass():
    _instance = None

    def __init__(self):
        raise RuntimeError('Call instance() instead')

    @classmethod
    def instance(cls):
        if cls._instance is None:
            print('Creating new instance')
            cls._instance = cls.__new__(cls)
        return cls._instance

singleton1 = SingletonClass.instance()
singleton1.my_variable = "my variable"
print(singleton1)
print(singleton1.my_variable)

singleton2 = SingletonClass.instance()
print(singleton2)
print(singleton2.my_variable)
```

```
Creating new instance
<__main__.SingletonClass object at 0x7f630cd08ca0>
my variable
<__main__.SingletonClass object at 0x7f630cd08ca0>
my variable
```

#### 3. Borg Singleton

singleton의 사용 목적인 해당 class에 대한 변수를 공유하는 것에 초점을 둔 것으로 선언되는 instance의 갯수에 대해서는 고려하지 않는 방법으로 아래와 같이 `class variable`을 활용하여 class의 속성정보를 저장하는 `__dict__` 변수를 이용한다. 

```python
class BorgSingletonClass():
    shared_variable = {}

    def __init__(self):
        self.__dict__ = BorgSingletonClass.shared_variable

singleton1 = BorgSingleton()
singleton1.my_var = 2
singleton1.my_str = "aaa"

print(singleton1)
print(singleton1.my_var)
print(singleton1.my_str)

singleton2 = BorgSingleton()
print(singleton2)
print(singleton2.my_var)
print(singleton2.my_str)
```

```
<__main__.BorgSingleton object at 0x7f87c5a30e50>
2
aaa
<__main__.BorgSingleton object at 0x7f87c5a30fd0>
2
aaa
```