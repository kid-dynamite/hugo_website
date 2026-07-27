+++
date = '2026-07-26T16:15:51+02:00'
draft = false
title = 'Recursion_simple'
showAuthor = false
weight = 50
layout = "simple"
summary = "🚀 simple recursion examples"
+++

> Recursion...

```python

# Returns the sum of an even number
# Error for odd numbers e.g. num = 7 -> num == 0 never reached/endless loop

def even_num(num):
    if num == 0:
        return 0
    return num + even_num(num - 2)

print(f"even_num(8): {even_num(8)}")    # prints: 20



# Returns the sum of a number
# Catches odd and negative numbers

def sum_num(num):
    if num <= 0:
        return 0
    return num + sum_num(num - 2)

print(sum_num(8))  # prints: 20
print(sum_num(7))  # prints: 16 (7 + 5 + 3 + 1)




# Returns the sum of a +int(including 0): factorial (num!)
# # 0! = 1

def factorial(num):
    if num == 0:
        return 1
    return num * factorial(num-1)

print(f"factorial(5): {factorial(5)}")      # prints: 120



# Walk 100 steps back

def walk(steps):
    if steps == 0:
        return
    print(f"You take step #{steps}")
    walk(steps - 1)

walk(100)   # prints: 100, 99, 98...



# Print a word

def print_chars(word: str, i: int) -> None:
    if i == len(word):
        return
    print(word[i])
    print_chars(word, i + 1)


print_chars("Hello", 0)
# H
# e
# l
# l
# o


# Countdown

def countdown(n: int) -> None:
    print(n)
    if n == 0:
        return
    else:
        countdown(n - 1)
```

> Iterative versions

```python

def sum_num_iterative(num):
    if num <= 0:
        return 0

    total = 0
    # counts backward from num down to 1 in steps of -2
    for i in range(num, 0, -2):
        total += i
    return total

print(sum_num_iterative(8))  # prints: 20
print(sum_num_iterative(7))  # prints: 16 (7 + 5 + 3 + 1)


def factorial_iterative(num):
    if num <= 0:
        return 1

    result = 1
    while num > 1:
        result *= num
        num -= 1  # decreases num by 1 in each step
    return result

print(factorial_iterative(5))  # prints: 120
```
