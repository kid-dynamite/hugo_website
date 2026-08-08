+++
date = '2026-07-26T22:58:22+02:00'
draft = false
title = 'Recursion_lists'
showAuthor = false
weight =52
layout = "simple"
summary = "🚀 list recursion"
+++

```python

# return the sum of a list

def sum_nums(nums: list[int]) -> int:
    if len(nums) == 0:
        return 0
    return nums[0] + sum_nums(nums[1:])


print(sum_nums([1, 2, 3, 4, 5]))        # prints: 15
```

> Find max in a list

```python
def max_nums(nums: list[int]) -> int:
    # Basis-Fall: Wenn nur noch ein Element da ist, ist es das Maximum
    if len(nums) == 1:
        return nums[0]
    if len(nums) == 0:
        return 0

    # Rekursiver Aufruf: Finde das Maximum im Rest der Liste (tail)
    max_of_rest = max_nums(nums[1:])

    # Vergleiche das erste Element mit dem Maximum des Rests
    print(nums)
    if nums[0] > max_of_rest:
        return nums[0]
    else:
        return max_of_rest

print(max_nums([1, 2, 3, 4, 5]))  # Ausgabe: 5
```

> Find max in a list

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int in a list
        ***Assume the list is not empty
        Ex: if lst = [9, 31,9], maxList(lst) returns 31
    """


    if len(lst) == 1:
        return lst[0]
    else:
        tempMax = maxList(lst[1:])
        return lst [0] if lst[0] > tempMax else tempMax
print(maxList([9, 31,9,7]))
```

> Find Max in nested list

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int in a list
        ***Assume the list is not empty
        Ex: if lst = [9, 31,9, "string", [99, 4, "er"]], maxList(lst) returns 99
    """

    if len(lst) == 0:
        return None
    else:
        tempMax = float("-inf")
        for item in lst:
            if isinstance(item, list):
                max_in_nested = maxList(item)
                if max_in_nested is not None and max_in_nested > tempMax:
                    tempMax = max_in_nested
            elif isinstance(item, int):
                if item > tempMax:
                    tempMax = item
        return tempMax if tempMax != float("-inf") else None

print(maxList([9, 31,9, "string", [99, 4, "er"]]))
```

### Ein konkretes Beispiel im Zeitraffen

Schauen wir uns an, was bei `maxList([9, [99, 4]])` passiert:

#### 1. Die Hauptfunktion startet (Runde 1)

- Sie sieht die `9`. Das ist ein `int`.
- `tempMax` der Hauptfunktion wird `9`.
- Jetzt kommt das nächste Element: `[99, 4]`. Das ist eine Liste!
- Die Hauptfunktion pausiert in der Zeile: `max_in_nested = maxList([99, 4])`

#### 2. Die Funktion startet neu für die innere Liste (Runde 2)

- Diese Runde hat ihr **eigenes, neues** `tempMax` (wieder gestartet bei -∞).
- Sie sieht `99` → ihr `tempMax` wird `99`.
- Sie sieht `4` → `99` bleibt größer.
- Die Liste ist zu Ende. Runde 2 erreicht die Zeile `return tempMax`.
- Da `tempMax` hier `99` ist, sagt die Funktion: **`return 99`**. Runde 2 schließt sich.

#### 3. Zurück in der Hauptfunktion (Runde 1)

- Der Aufruf `maxList([99, 4])` wird nun durch das Ergebnis `99` ersetzt.
- Die Zeile liest sich jetzt so:
  ```python
  max_in_nested = 99
  ```
- **Jetzt** hat `max_in_nested` den Wert `99` gespeichert!
- Erst danach folgt der Vergleich in der Hauptfunktion:
  ```python
  if max_in_nested > tempMax:  # Ist 99 > 9? Ja!
      tempMax = max_in_nested  # Haupt-tempMax wird 99
  ```

### Zusammenfassung

`max_in_nested` selbst sucht nicht nach Zahlen. Es ist wie ein **Postbote**. Es wartet geduldig, bis der rekursive Aufruf (`maxList(item)`) fertig geschaut hat, nimmt das Endergebnis (den `return`-Wert) entgegen und hält es für den anschließenden Vergleich bereit.

```python
def maxList(lst):
    """
    Use Recursion to Return the maximum value in the list
    """
    if len(lst) == 0:
        return None
    else:
        tempMax = float("-inf")
        for item in lst:
            # Das if-Statement wird bei JEDEM Element als Erstes abgefragt:
            print(f"--> Abfrage 'if': Ist '{item}' eine Liste?")

            if isinstance(item, list):
                print(f"    JA! '{item}' ist eine Liste. Ich springe tiefer hinein...\n")
                max_in_nested = maxList(item)

                print(f"    <-- Zurück aus der Unterliste. Gefundenes Max dort war: {max_in_nested}")
                if max_in_nested is not None and max_in_nested > tempMax:

                    tempMax = max_in_nested

            elif isinstance(item, int):
                print(f"    NEIN! Aber '{item}' ist eine Zahl. Vergleiche mit tempMax...")
                if item > tempMax:
                    tempMax = item
                    print(f"    => Neues tempMax ist: {tempMax}")

        return tempMax if tempMax != float("-inf") else None

# Starte den Testlauf
print("=== START DES PROGRAMMS ===")
ergebnis = maxList([9, 31, 9, "string", [99, [201, [501]], 4, "er"]])
print("===========================")
print(f"Endergebnis: {ergebnis}")
```

> Find Max in a nested list --> real recursion

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int in a list
        ***Assume the list is not empty
        Ex: if lst = [9, 31,9, "string", [99, 4, "er"]], maxList(lst) returns 99
    """

    if not lst:
        return 0

    head = lst[0]
    tail = maxList(lst[1:])

    num = 0

    if isinstance(head, int):
        if head > tail:
            return head
        else:
            return tail
    if isinstance(head, list):
        return maxList(head)
    else:
        return tail
    return tail

print(maxList([9, 31,9, "string", [99, 4, "er"]]))

```

> Count strings in a nested list

```python
def count_strings(lst):
    # Passe den Basis-Fall an: Was wird bei einer leeren Liste zurückgegeben?
    if not lst:
        return 0

    head = lst[0]
    tail = count_strings(lst[1:])

    if isinstance(head, str):
        return 1 + tail

    if isinstance(head, list):
        return count_strings(head) + tail


    return tail

    # Ergänze hier deine rekursive Logik und die Typ-Prüfungen




# Test-Aufruf (Das erwartete Ergebnis für diese Liste ist 4)
values = [1, "Apfel", [2, "Banane"], "Orange", [3, [4, "Erdbeere"]]]
result = count_strings(values)

print(result)
# Erwartete Ausgabe: 4
```

> Some math in nested lists --> modulo

```python
def sum_even_numbers(lst):
    # Basis-Fall: Was gibt eine leere Liste beim Zusammenrechnen als Basis zurück?
    if not lst:
        return 0

    head = lst[0]
    tail = sum_even_numbers(lst[1:])


    # Ergänze hier deine rekursive Logik und die Prüfungen
    if isinstance(head, int):
        if head % 2 == 0:
            return head + tail
    if isinstance(head, list):
        return sum_even_numbers(head) + tail


    return tail





# Test-Aufruf (Erwartetes Ergebnis: 2 + 4 + 6 = 12)
# Die 3, 5 und "Hallo" müssen ignoriert werden!
values = [2, 3, [4, "Hallo"], 5, [6]]
result = sum_even_numbers(values)

print(result)
# Erwartete Ausgabe: 12
```

```python

# Calc the sum of all int i a list

def sum_list_ints(lst):
    add_int = 0
    for item in lst:
        if isinstance(item, list):
            # Rekursiver Aufruf für verschachtelte Listen
            add_int += sum_list_ints(item)
        elif isinstance(item, int) and not isinstance(item, bool):
            # Da bool ein Untertyp von int ist, schließt dies True/False aus
            add_int += item

    return add_int

print(sum_list_ints([9, 31, 9, "string", [99, 4, "er"]]))  # Ausgabe: 152
```
