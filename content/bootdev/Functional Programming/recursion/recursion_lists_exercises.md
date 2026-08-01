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

Das Missverständnis liegt oft daran, wie man sich die Funktion vorstellt. Du musst dir `sum_list_ints` wie eine Schablone vorstellen. Jedes Mal, wenn die Funktion aufgerufen wird, wird eine völlig neue, eigenständige Kopie dieser Schablone im Arbeitsspeicher gestartet.

Jede Kopie hat ihr eigenes `total` und ihr eigenes `return`.

Hier ist der genaue Weg, wie der Wert nach oben geschickt wird:

### 1. Die innere Kopie (für die Unterliste)

Wenn die Hauptliste bei der Unterliste `[99, 4, "er"]` ankommt, wird eine zweite, innere Kopie der Funktion gestartet. Diese innere Kopie rechnet 99 + 4 und ihr `total` steht am Ende auf **103**.

Jetzt erreicht diese innere Kopie den `return total`-Befehl ganz unten.

**Das passiert beim Ausführen:** Sie schickt die `103` hoch an die Stelle, von der sie aufgerufen wurde. Danach löst sich diese innere Kopie im Arbeitsspeicher auf.

### 2. Die äußere Kopie (für die Hauptliste)

Die äußere Kopie hat die ganze Zeit gewartet. Sie stand genau an dieser Zeile:

```python
list_sum = sum_list_ints(i)  # Hier kommt die 103 von unten "angeflogen"
```

Die äußere Kopie fängt die `103` ab und speichert sie in `list_sum`. Sie rechnet $49 + 103 = 152$.

Erst jetzt erreicht auch die äußere Kopie ganz am Ende ihren eigenen `return total`-Befehl. Sie schickt die `152` hoch an dein `print()`.

### Zusammenfassung

Der `return`-Befehl ganz unten im Code wird in diesem Beispiel **exakt zweimal** ausgeführt:

- **Das erste Mal** von der inneren Kopie: Schickt die `103` hoch zur Hauptschleife.
- **Das zweite Mal** von der äußeren Kopie: Schickt die finale `152` hoch zum `print`-Befehl.
