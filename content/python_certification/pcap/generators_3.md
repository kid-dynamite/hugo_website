+++
date = '2026-08-16T12:00:59+02:00'
draft = false
title = 'Python_generators_3'
showAuthor = false
weight =25
layout = "simple"
summary = "🚀 Python_generators_3"
+++

```python
import sys

"""
def gen(n):
    for i in range(n ):
        yield i

# yield pauses the execution of the function and
# returns the value to whoever is iterating through
# this generator object

#for i in gen(5):
    #print(i)

x = gen(5)
print(next(x))
print(next(x))
print(next(x))
"""

def gen():
    yield 1
    print("Pause 1")
    yield 2
    print("Pause 2")
    yield 3
    print("Pause 3")
    yield 4

x = gen()
print(next(x))
print(next(x))
print(next(x))
print(next(x))


# generator comprehension

x = (i for i in range(10))

print(x)    # <generator object <genexpr> at 0x000002742968FAC0>

print(next(x))  # 0
print(next(x))  # 1
print(next(x))  # 2
print(next(x))  # 3

#for j in x:
    #print(j)

```

> Telusko_1

```Python
nums = [7, 8, 9, 5]

it = iter(nums)

print(it.__next__())    # 7

print(next(it))         # 8

for i in nums:
    print(i)

    # 7, 8...
```

> Telusko_2

```python
def top5():

    yield 1
    yield 2
    yield 3
    yield 4
    yield 5

values = top5()

print(values)   # <generator object topten at 0x000001ED7256E140>

print(values.__next__())    # 1
print(values.__next__())    # 2
print(values.__next__())    # 3

for i in values:            # 4, 5
    print(i)

print("*****************************************")

def topten():

    n = 1

    while n <= 10:
        sq = n*n
        yield sq
        n += 1

values = topten()

for i in values:
    print(i)

"""
1
4
9
16
25
36
49
64
81
100
"""

```
