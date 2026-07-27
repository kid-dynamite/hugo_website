+++
date = '2026-07-26T22:53:47+02:00'
draft = false
title = 'Recursion_fibonacci'
showAuthor = false
weight =51
layout = "simple"
summary = "🚀 fibonacci recursion"
+++

> Iterative Approach (for loop)

```python
# Fibonacci sequence:   0, 1, 1, 2, 3, 5, 8, 13, ...
#               n:      0, 1, 2, 3, 4, 5, 6, 7

def fibonacci_iterative(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1

    a, b = 0, 1
    # Loop runs from 2 up to n
    for _ in range(2, n + 1):
        a, b = b, a + b  # a gets the value of b; b gets the sum of both
    return b

print(fibonacci_iterative(6))  # prints: 8
print(fibonacci_iterative(7))  # prints: 13
```

> Recursive Approach

```python
def fibonacci_recursive(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    # Returns the sum of the two previous Fibonacci numbers
    return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2)

print(fibonacci_recursive(6))  # prints: 8
```
