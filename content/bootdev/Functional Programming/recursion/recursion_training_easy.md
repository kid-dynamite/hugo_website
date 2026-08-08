+++
date = '2026-08-03T21:04:13+02:00'
draft = false
title = 'Recursion_boot.dev_training_easy'
showAuthor = false
weight =57
layout = "simple"
summary = "🚀 simple recursion exercises from boot.dev"
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

### 3. Das Zusammenfügen (Der Rückweg)

Jetzt prüft die Funktion bei jedem Schritt: Ist der aktuelle erste Buchstabe (`text[0]`) derselbe wie der erste Buchstabe des bereits bereinigten Rests (`remaining[0]`)? Wenn ja, wird der aktuelle Buchstabe ignoriert. Wenn nein, wird er vorne angehängt.

- **Schritt für `"er"`:** `text[0]` ist `"e"`, `remaining` ist `"r"`.
  - `"e" != "r"` → Rückgabe: `"e" + "r"` = **`"er"`**
- **Schritt für `"per"`:** `text[0]` ist `"p"`, `remaining` ist `"er"`.
  - `"p" != "e"` → Rückgabe: `"p" + "er"` = **`"per"`**
- **Schritt für `"eper"`:** `text[0]` ist `"e"`, `remaining` ist `"per"`.
  - `"e" != "p"` → Rückgabe: `"e" + "per"` = **`"eper"`**
- **Schritt für `"eeper"` (Erstes Duplikat):** `text[0]` ist `"e"`, `remaining` ist `"eper"`.
  - `"e" == "e"` (Buchstaben gleich!) → Das aktuelle `"e"` wird ignoriert. Rückgabe nur `remaining`: **`"eper"`**
- **Schritt für `"keeper"`:** `text[0]` ist `"k"`, `remaining` ist `"eper"`.
  - `"k" != "e"` → Rückgabe: `"k" + "eper"` = **`"keeper"`**
- **Schritt für `"kkeeper"` (Zweites Duplikat):** `text[0]` ist `"k"`, `remaining` ist `"keeper"`.
  - `"k" == "k"` (Buchstaben gleich!) → Das aktuelle `"k"` wird ignoriert. Rückgabe: **`"keeper"`**
- **Schritt für `"okkeeper"`:** `text[0]` ist `"o"`, `remaining` ist `"keeper"`.
  - `"o" != "k"` → Rückgabe: `"o" + "keeper"` = **`"okeeper"`**
- **Schritt für `"ookkeeper"` (Drittes Duplikat):** `text[0]` ist `"o"`, `remaining` ist `"okeeper"`.
  - `"o" == "o"` (Buchstaben gleich!) → Das aktuelle `"o"` wird ignoriert. Rückgabe: **`"okeeper"`**
- **Schritt für `"bookkeeper"`:** `text[0]` ist `"b"`, `remaining` ist `"okeeper"`.
  - `"b" != "o"` → Rückgabe: `"b" + "okeeper"` = **`"bokeper"`**

```python
def count_occurrences(values, target):
    # SCHRITT 1: Die Notbremse (Wenn die Liste leer ist, haben wir 0 Treffer)
    if len(values) == 0:
        return 0

    # SCHRITT 2: Der Rekursions-Aufruf (Wir holen uns das Ergebnis vom Rest der Liste)
    anzahl_im_rest = count_occurrences(values[1:], target)

    # SCHRITT 3: Die Prüfung (Wir schauen uns NUR das allererste Element an)
    if values[0] == target:
        return anzahl_im_rest + 1  # Wenn es passt: Rest + 1
    else:
        return anzahl_im_rest      # Wenn nicht: Nur der Rest



print(count_occurrences(["potion", "key", "potion"], "potion"))
```

> It should recursively search values from left to right and return the index of the first item equal to target. Return -1 when the target is absent.

```python
def find_first_index(values, target, current_index=0):
    if current_index >= len(values):
        return -1
    if values[current_index] == target:
        return current_index
    return find_first_index(values, target, current_index + 1)


find_first_index(["idle", "alert", "alert"], "alert")
# 1

find_first_index([4, 8, 12], 7)
# -1
```

> It should recursively return the largest number in a non-empty list

```python
def largest_number(numbers):
    if len(numbers) == 1:
        return numbers[0]

    large_num = 0
    head = numbers[0]
    tail = largest_number(numbers[1:])

    if head > tail:
        return head
    else:
        return tail

```
