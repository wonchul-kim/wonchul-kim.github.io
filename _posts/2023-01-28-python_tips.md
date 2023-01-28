---
layout: post
title: Tips
category: Python
tag: python tips
---

0. Try to follow [PEP 8](https://peps.python.org/pep-0008/) guides

1. Try NOT to use `import *`

* 해당 module의 모든 object를 로딩할 수 있기 때문에 비효율적
* object 명이 동일해서 겹칠 수가 있는 위험이 존재

2. Try to specifiy `except ...`

```python
# Try - except
# Bad
try:
    driver.find_element(...)
except:
    print("Which exception?")

# Good
try:
    driver.find_element(...)
except NoSuchElementException:
    print("It's giving NoSuchElementException")
except ElementClickInterceptedException:
    print("It's giving ElementClickInterceptedException")
```

3. Try to use `numpy` for mathmatical calculation

어떤 백터 계열의 계산에 있어서 `for ...`문을 사용하기 보다는 `numpy`를 활용하는 것이 더 빠르다. 
```python
import numpy as np

random_scores = np.random.randint(1, 100, size=10000001)

# Bad (solving problem with a for loop)
count_failed = 0
sum_failed = 0
for score in random_scores:
    if score < 70:
        sum_failed += score
        count_failed += 1
print(sum_failed/count_failed)

# Good (solving problem using vector operations)
mean_failed = (random_scores[random_scores < 70]).mean()
print(mean_failed)
```

4. Try to do `close` if possible

예를 들어, file, matplotlib, ...

5. Try to use **comprehension**
```python
# Bad
countries = ['USA', 'UK', 'Canada']

lower_case = []
for country in countries:
    lower_case.append(country.lower())

# Good (but don't overuse it!)
lower_case = [country.lower() for country in countries]
```

6. Try to use **f-string**

https://towardsdatascience.com/python-f-strings-are-more-powerful-than-you-might-think-8271d3efbd7d




#### references
* [10 Python Mistakes That Tell You’re a Nooby](https://medium.com/geekculture/10-python-mistakes-that-tell-youre-a-nooby-359487f22c97)