+++
date = '2026-08-03T21:04:13+02:00'
draft = false
title = 'Recursion_dict_training_easy'
showAuthor = false
weight =57
layout = "simple"
summary = "🚀 simple recursion exercises"
+++

```python

def is_palindrome(text):
if len(text) <= 1:
return True
if text[0] != text[-1]:
return False
return is_palindrome(text[1:-1])

is_palindrome("racecar")
# True

is_palindrome("python")
# False

```

```python
def collapse_repeats(text):
    if len(text) <= 1:
        return text

    remaining = collapse_repeats(text[1:])
    if text[0] == remaining[0]:
        return remaining
    return text[0] + remaining


collapse_repeats("boookkeeper")
# "bokeper"

collapse_repeats("abca")
# "abca"
```
