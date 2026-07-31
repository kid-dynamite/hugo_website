+++
date = '2026-07-31T15:58:02+02:00'
draft = false
title = 'Recursion_lists_exercises'
showAuthor = false
weight =55
layout = "simple"
summary = "🚀 list recursion exercises"
+++

```python

# return the sum of a list

def sum_nums(nums: list[int]) -> int:


print(sum_nums([1, 2, 3, 4, 5]))        # prints: 15
```

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int the list
        ***Assume hte list is not empyt
        Ex: if lst = [9, 31,9], maxList(lst) returns 31
    """



print(maxList([9, 31,9,7]))
```

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int the list
        ***Assume hte list is not empyt
        Ex: if lst = [9, 31,9, "string", [99, 4, "er"]], maxList(lst) returns 99
    """


print(maxList([9, 31,9, "string", [99, 4, "er"]]))
```

```python

# Calc the sum of all int i a list

def sum_list_ints(lst):


print(sum_list_ints([9, 31, 9, "string", [99, 4, "er"]]))  # Ausgabe: 152
```
