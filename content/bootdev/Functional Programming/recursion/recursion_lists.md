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
