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
"""
num = 8: 8 + sum_calc(6)
num 2 = 2 + 0 = 2
num 4 = 4 + 2 = 6
num 6 = 6 + 6 = 12
num 8 = 8 + 12 = 20
"""
print(sum_num(7))  # prints: 16 (7 + 5 + 3 + 1)
```

> Factorial

```python
# Returns the sum of a +int(including 0): factorial (num!)
# # 0! = 1

def factorial(num):
    if num == 0:
        return 1
    return num * factorial(num-1)

print(f"factorial(5): {factorial(5)}")      # prints: 120
```

So baut sich der Weg zu den 120 Schritt für Schritt auf:

### 1. Die Abbau-Phase (Der Stack baut sich auf)

Der Computer rechnet noch gar nichts aus, sondern schachtelt die Aufgaben erst einmal so lange ineinander, bis er ganz unten ankommt:

- `fakultaet(5)` wird zu: `5 * fakultaet(4)`
- `fakultaet(4)` wird zu: `4 * fakultaet(3)`
- `fakultaet(3)` wird zu: `3 * fakultaet(2)`
- `fakultaet(2)` wird zu: `2 * fakultaet(1)`
- `fakultaet(1)` erreicht den Basisfall und gibt einfach nur `1` zurück.

### 2. Die Auflösungs-Phase (Die Multiplikation)

Jetzt, wo der Computer weiß, dass `fakultaet(1) = 1` ist, setzt er die Zahlen von unten nach oben wieder rückwärts ein und multipliziert sie:

- `fakultaet(2)` wird zu: `2 * 1 = 2`
- `fakultaet(3)` wird zu: `3 * 2 = 6`
- `fakultaet(4)` wird zu: `4 * 6 = 24`
- `fakultaet(5)` wird zu: `5 * 24 = 120`

Am Ende steht in der innersten Logik also die reine Multiplikationskette:
`5 * 4 * 3 * 2 * 1 = 120`.

> Walk 100 steps back

```python
def walk(steps):

    if steps == 0:
        return

    print(f"You take step #{steps}")
    walk(steps - 1)


#walk(100) # prints: 100, 99, 98...

# ********************************************

def walk(steps):

    if steps == 0:
        return # or return 0

    walk(steps - 1)
    print(f"You take step #{steps}")

walk(100) # prints: 1, 2, ..., 100
```

```python
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
