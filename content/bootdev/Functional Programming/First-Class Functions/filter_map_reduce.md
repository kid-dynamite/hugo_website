+++
date = '2026-07-31T21:56:44+02:00'
draft = false
title = 'Lambda, Filter, Map, Reduce'
showAuthor = false
weight =50
layout = "simple"
summary = "🚀 simple examples of lambda, filter and reduce"
+++

```python
from functools import reduce

#def is_even(n):
    #return n%2 == 0

nums = [3, 2, 6, 8, 4, 6, 2, 9]

#evens = list(filter(is_even, nums))
evens = list(filter(lambda n : n%2 == 0, nums))

doubles = list(map(lambda n : n*2, evens))

sum = reduce(lambda a, b : a+b, doubles)

print(evens)
print(doubles)
print(sum)

```
