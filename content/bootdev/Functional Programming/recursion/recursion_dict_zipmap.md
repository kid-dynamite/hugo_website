+++
date = '2026-07-27T00:27:54+02:00'
draft = false
title = 'Recursion_dic_zipmap'
showAuthor = false
weight =53
layout = "simple"
summary = "🚀 recursion on a dictionary, keys and values provided by lists"
+++

> Exercise
> ![zipmap_recursion](/img/recursion_dictionary_zipmap.jpg)

> Solution

```Python
def zipmap(keys: list[str], values: list[float]) -> dict[str, float]:
    if len(keys) == 0 or len(values) == 0:
        return {}
    res = zipmap(keys[1:], values[1:])
    res[keys[0]] = values[0]
    return res
```
