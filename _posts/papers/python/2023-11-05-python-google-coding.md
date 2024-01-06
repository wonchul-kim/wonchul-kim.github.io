---
layout: post
title: Google Coding Style
category: Python
tag: google coding style
---


### Walrus Operator (:=)

```python
value = custom_function()
if value > 10:
    print(value)
```

to

```python
if (value := custom_function()) > 10:
    print(value)
```

### List Comprehensions

```python
numbers = [1, 2, 3, 4]
squared_even_numbers = []
for number in numbers:
    if number%2 == 0:
        squared_even_numbers.append(number**2)
```

to

```python
squared_even_numbers = [number**2 for number in numbers if number%2 == 0]
```

This is not only concise but also <u>computationally fatser</u>

### `map` and `filter`

For complex transformations, `map` and `filter` can be faster than **list comprehensions**.

The `map` function makes us to iterate all items of iterable for a given function:
```python
squared_numbers = list(map(lambda x: x**2, numbers))
```

The `filter` function applies certain conditions:
```python
even_numbers = list(filter(lambda x: x%2 == 0, numbers))
```

### Nested Loops

#### Generators: Lazy Loading

For a loop of large datasets, generator can be <u>memory-efficient</u>.

```python
def count_up_to(max):
    count = 1
    while count <= max:
        yield count
        count += 1

counter = count_up_to(5)
```


### Magical Libraries Only Insiders Use

#### 1. `Faker`

```python
from faker import Faker 
fake = faker()
```

#### 2. `Dash`
```python
import dash
import dash_core_components as dcc
import dash_html_components as html

app = dash.Dash(__name__)

app.layout = html.Div([
    dcc.Graph(figure={'data': [{'x': [1, 2, 3], 'y': [1, 3, 2], 'type': 'bar'}]})
])

app.run_server(debug=True)
```

#### 3. `Arrow`

```python
import arrow

utc = arrow.utcnow()
local = utc.to('US/Pacific')

print(local.format('YYYY-MM-DD HH:mm:ss'))
```

#### 4. `joblib`
```python
from joblib import dump, load

# Save a model
dump(model, 'model.joblib')

# Load a model
model = load('model.joblib')
```

#### 5. `pydantic`
