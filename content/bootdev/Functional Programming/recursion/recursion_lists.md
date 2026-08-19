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
    if not lst:
        return float('-inf')

    head = lst[0]
    tail_max = maxList(lst[1:])

    # NEU: Wenn head eine Liste ist, packen wir sie aus und suchen ihr Maximum
    if isinstance(head, list):
        # Wir überschreiben head mit der größten Zahl aus DIESER inneren Liste
        head = maxList(head)
        # (Bei [99, 4, "er"] wird head hier drin zu der Zahl 99)

    # Ab hier ist alles EXAKT so, wie du es gerade erklärt hast:
    if isinstance(head, int):
        if head > tail_max:
            return head
        else:
            return tail_max
    else:
        return tail_max


# Testet perfekt und gibt 99 aus
print(maxList([9, 31, 9, "string", [99, 4, "er"], 500]))

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

> Calc the sum of all int i a list

```python


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

> Return depth of a list with for-loop and recursion

```python
def maxDepth(lst):
    if not isinstance(lst, list):
        return 0

    current_max = 1  # Die aktuelle Liste selbst zählt als Ebene 1

    for item in lst:
        if isinstance(item, list):
            # Hier wird das +1 direkt für die nächste Ebene addiert
            sub_depth = 1 + maxDepth(item)
            if sub_depth > current_max:
                current_max = sub_depth

    return current_max

```

> another way

```python
def maxDepth(lst):
    """
    Nutzt Rekursion, um die maximale Tiefe einer Liste zu berechnen.
    Eine flache Liste wie [1, 2, 3] hat die Tiefe 1.
    """
    if not isinstance(lst, list):
        return 0

    max_sub_depth = 0
    for item in lst:
        if isinstance(item, list):
            sub_depth = maxDepth(item)
            if sub_depth > max_sub_depth:
                max_sub_depth = sub_depth

    return 1 + max_sub_depth

# Test mit Ihrem Beispiel
print(maxDepth([9, 31, 9, "string", [99, 4, "er"]])) # Gibt 2 aus
print(maxDepth([1, [2, [3, [4]]]]))                  # Gibt 4 aus

```

> return a new list without None

```python
def remove_none_values(data):
    if not data:
        return []

    head = data[0]
    tail = remove_none_values(data[1:])

    if isinstance(head, list):
        # Wichtig: head rekursiv reinigen UND in eine Liste packen [[...]],
        # damit die Struktur erhalten bleibt, wenn wir es mit tail verbinden!
        return [remove_none_values(head)] + tail
    elif head is None:
        # Wenn es None ist, werfen wir es weg und geben nur den Rest zurück
        return tail
    else:
        # Für alle anderen gültigen Werte (wie int, str etc.):
        # Wir packen head in eine Liste [head] und hängen tail hinten dran
        return [head] + tail

# TESTFÄLLE
list_1 = [1, None, 3]
print(remove_none_values(list_1))
# Ausgabe: [1, 3]

list_2 = [1, [2, None, 3], None, [None, [4, 5]]]
print(remove_none_values(list_2))
# Ausgabe: [1, [2, 3], [[4, 5]]]
```

> with for-loop and recursion

```python
def remove_none_values(data):
    # Basis-Fall: Falls die Liste komplett leer ist
    if not data:
        return []

    result = []

    # Die Schleife läuft flach durch die aktuelle Ebene
    for item in data:
        if isinstance(item, list):
            # REKURSION: Wenn es eine Liste ist, tauchen wir ab,
            # reinigen sie und hängen die gereinigte Liste an result an.
            cleaned_nested = remove_none_values(item)
            result.append(cleaned_nested)
        elif item is not None:
            # Normale Werte (int, str etc.) werden einfach behalten
            result.append(item)
        # Wenn item None ist, springt die Schleife weiter (wird ignoriert)

    return result

# TESTFÄLLE
list_1 = [1, None, 3]
print(remove_none_values(list_1))
# Ausgabe: [1, 3]

list_2 = [1, [2, None, 3], None, [None, [4, 5]]]
print(remove_none_values(list_2))
# Ausgabe: [1, [2, 3], [[4, 5]]]
```

> Create a new list with all the strings

```python
def extract_all_text(dom_tree):
    result = []

    for item in dom_tree:
        if isinstance(item, str):
            result.append(item)
        if isinstance(item, list):
            scan_list = extract_all_text(item)
            result.extend(scan_list)


    return result

# TESTFÄLLE

# Ein einfaches Dokument: Eine Überschrift und ein Absatz
site_1 = ["Willkommen", "Auf meiner Website", ["Hier ist Text"]]
print(extract_all_text(site_1))
# Erwartete Ausgabe: ['Willkommen', 'Auf meiner Website', 'Hier ist Text']

# Ein komplexes Dokument mit Zahlen (IDs) und tiefen Verschachtelungen
site_2 = ["Startseite", 1024, ["Menü", ["Home", "Über uns"]], "Footer-Text", 404]
print(extract_all_text(site_2))
# Erwartete Ausgabe: ['Startseite', 'Menü', 'Home', 'Über uns', 'Footer-Text']

```

> Calculate the sum of all ints in a nested list

```python
def calculate_disk_space(folder):
    total_size = 0

    for item in folder:
        if isinstance(item, int):
            total_size += item
        if isinstance(item, list):
            sum_list = calculate_disk_space(item)
            total_size += sum_list





    return total_size

# TESTFÄLLE

# Ein einfacher Ordner mit 3 Dateien
folder_1 = [12, 45, 5]
print(calculate_disk_space(folder_1))
# Erwartete Ausgabe: 62

# Ein verschachtelter Ordner (Hauptordner hat Dateien und zwei Unterordner)
folder_2 = [10, [20, 30], 5, [1, [2, 3]]]
print(calculate_disk_space(folder_2))
# Erwartete Ausgabe: 71
```

```python
def extract_all_text(dom_tree, depth=0):  # depth startet standardmäßig bei 0
    result = []

    for item in dom_tree:
        if isinstance(item, str):
            str_tuple = (item, depth)  # Nutzt die aktuelle Tiefe
            result.append(str_tuple)
        elif isinstance(item, list):
            # Wir tauchen ab und sagen der tieferen Ebene: "Du bist eine Stufe tiefer (+1)!"
            scan = extract_all_text(item, depth + 1)
            result.extend(scan)

    return result


# TESTFÄLLE (Die Ausgabe liefert dir jetzt den Text UND die exakte HTML-Tiefe!)

site_1 = ["Willkommen", "Auf meiner Website", ["Hier ist Text"]]
print(extract_all_text(site_1))
# Ausgabe: [('Willkommen', 0), ('Auf meiner Website', 0), ('Hier ist Text', 1)]

site_2 = ["Startseite", 1024, ["Menü", ["Home", "Über uns"]], "Footer-Text", 404]
print(extract_all_text(site_2))
# Ausgabe: [('Startseite', 0), ('Menü', 1), ('Home', 2), ('Über uns', 2), ('Footer-Text', 0)]
```
