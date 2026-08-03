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

> Flatten a list

```python
# Dein Test-Datensatz für die "Flatten"-Aufgabe
fiese_liste = [
    1, ,
    [],                                  # Eine leere Liste (sollte verschwinden)
    "string1",
    [4, ["string2", 5, [6, 7]], 8],      # Tiefe Verschachtelung
    [9]
]

# Das gewünschte Ergebnis nach deinem .extend()/.append() Umbau:
# [1, 2, 3, "string1", 4, "string2", 5, 6, 7, 8, 9]


# Hier kannst du später deine Funktion testen:
def flatten_list(lst):
    # Dein Code kommt hier hin...
    pass

# print(flatten_list(fiese_liste))

```

> Solution

```python
def flatten_list(nested_list):
    # Basisfall 1: Die Liste ist leer
    if not nested_list:
        return []

    head = nested_list[0]
    tail = nested_list[1:]

    # Basisfall 2: Das erste Element ist eine Liste -> beide Teile rekursiv flachklopfen
    if isinstance(head, list):
        return flatten_list(head) + flatten_list(tail)

    # Rekursiver Schritt: Erstes Element ist eine Zahl -> mit dem flachen Rest verbinden
    return [head] + flatten_list(tail)

# Testläufe
print(flatten_list([1, [2, 3], 4]))       # Ausgabe: [1, 2, 3, 4]
print(flatten_list([1, [2, [3, 4]], 5])) # Ausgabe: [1, 2, 3, 4, 5]
```

> another Solution

```python
def flatten_list(lst):
    # 1. Dein korrekter Basisfall
    if len(lst) == 0:
        return []

    # Wir nehmen uns NUR das erste Element und den Rest
    head = lst[0]
    tail = lst[1:]

    # 2. Wenn das erste Element eine Zahl ist (wie dein isinstance)
    if isinstance(head, int):
        # Wir packen die Zahl in eine Liste und hängen den
        # rekursiv gelösten Rest hinten dran.
        return [head] + flatten_list(tail)

    # 3. Wenn das erste Element eine Liste ist
    if isinstance(head, list):
        # Wir klopfen die innere Liste flach UND den Rest der Liste
        return flatten_list(head) + flatten_list(tail)
```
