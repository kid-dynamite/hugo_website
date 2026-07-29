+++
date = '2026-07-29T23:20:05+02:00'
draft = false
title = 'Recursion_dict_calc_sum'
showAuthor = false
weight =55
layout = "simple"
summary = "🚀 calculate sum of dictionary values by recursion"
+++

> Exercise

```python
# Werte dieses Dictionaries, die aufsummiert werden sollen:
# 50 + 15 + 25 + 10 + 30 = 130
inventory = {
    "gold": 50,
    "weapons": {
        "swords": 15,
        "axes": 25
    },
    "potions": {
        "health": {
            "small": 10,
            "large": 30
        }
    }
}

# Deine Aufgabe: Schreibe die Funktion sum_dict_values(d)
# Tipp: Nutze wieder `if isinstance(v, dict):` für den rekursiven Aufruf!
```

> Solution

```python
def sum_dict_values(d):
    # Falls das Element kein Dictionary ist (z.B. eine Zahl),
    # brechen wir ab und geben direkt den Wert zurück.
    if not isinstance(d, dict):
        return d

    total_sum = 0

    # Wir gehen durch alle Werte des aktuellen Dictionaries
    for v in d.values():
        if isinstance(v, dict):
            # Wenn der Wert ein Unter-Dictionary ist, tauchen wir rekursiv ab
            total_sum += sum_dict_values(v)
        else:
            # Wenn es eine normale Zahl ist, addieren wir sie einfach
            total_sum += v

    return total_sum

# Testlauf
inventory = {
    "gold": 50,
    "weapons": {
        "swords": 15,
        "axes": 25
    },
    "potions": {
        "health": {
            "small": 10,
            "large": 30
        }
    }
}

ergebnis = sum_dict_values(inventory)
print(f"Die Gesamtsumme aller Werte beträgt: {ergebnis}")  # Sollte 130 ausgeben
```
