+++
date = '2026-08-08T19:24:26+02:00'
draft = false
title = 'Recursion_boot.dev_training_medium'
showAuthor = false
weight =61
layout = "simple"
summary = "🚀 recursion exercises on boot.dev training ground - medium"
+++

> Replace values in a list (replace 1 by 9)

```python
def replace_nested(values, target, replacement):
    if values == []:
        return []

    # HIER war der Fehler: Es muss values[0] sein, um an das Element zu kommen!
    head = values[0]
    tail = values[1:]

    if isinstance(head, list):
        replaced_head = replace_nested(head, target, replacement)

    elif head == target:
        replaced_head = replacement

    else:
        replaced_head = head

    # Am Ende kleben wir das fertige Element an den rekursiv verarbeiteten Rest
    return [replaced_head] + replace_nested(tail, target, replacement)


values = [1, [2, 1], [3, [1]]]
result = replace_nested(values, 1, 9)

print(result)
# [9, [2, 9], [3, [9]]]

#print(values)
# [1, [2, 1], [3, [1]]]
```
