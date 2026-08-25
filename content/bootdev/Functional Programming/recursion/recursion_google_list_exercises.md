+++
date = '2026-08-06T20:31:50+02:00'
draft = false
title = 'Recursion_google_list_exercises'
showAuthor = false
weight =59
layout = "simple"
summary = "🚀 simple google  exercises on list recursion"
+++

> Vokale zaehlen

```python
def zaehle_vokale(text):
    # Dein Code hier
    if not text:
        return 0

    head = text[0]
    tail = zaehle_vokale(text[1:])
    vokale = ["a","e","i","o","u"]

    if head in vokale:
        return 1 + tail
    else:
        return tail

# Hier rufst du die Funktion mit dem Text "python" auf:
print(zaehle_vokale("python"))

```

> Verdoppele alle Zahlen einer Liste

```python
def verdopple_liste(zahlen: list[int]) -> list[int]:

    #Verdoppelt rekursiv alle Zahlen in einer Liste.
    #Beispiel: verdopple_liste([1, 2, 3]) soll [2, 4, 6] zurückgeben.


    if not zahlen:
        return []

    verdoppelter_rest = verdopple_liste(zahlen[1:])

    ergebnis_zahl = zahlen[0] * 2

    return [ergebnis_zahl] + verdoppelter_rest





# Teste deinen Code hier:
test_liste = [1, 2, 3, 4]
print(verdopple_liste(test_liste))  # Sollte [2, 4, 6, 8] ausgeben
```

> Listen filtern

```python
def filter_a_woerter(woerter: list[str]) -> list[str]:

    #Filtert rekursiv eine Liste und gibt nur Wörter zurück, die mit 'A' oder 'a' starten.
    #Beispiel: filter_a_woerter(["Apfel", "Birne", "Ananas"]) -> ["Apfel", "Ananas"]


    if not woerter:
        return[]

    tail = filter_a_woerter(woerter[1:])


    if woerter[0].lower().startswith('a') or woerter[0].lower().startswith('A'):
        return [woerter[0]] + tail
    else:
        return tail


# Teste deinen Code hier:
test_liste = ["Apfel", "Banane", "Erdbeere", "Ananas", "Auto", "Hund"]
print(filter_a_woerter(test_liste))  # Sollte genau ["Apfel", "Ananas", "Auto"] ausgeben
```

> Ersetzen von Listen-Elementen

```python
def ersetze_zahl(zahlen: list[int], alt: int, neu: int) -> list[int]:
    """
    Ersetzt rekursiv alle Vorkommen von 'alt' durch 'neu' in der Liste.
    Beispiel: ersetze_zahl([1, 2, 1, 3], alt=1, neu=9) -> [9, 2, 9, 3]
    """
    # 1. Basis-Fall: Wenn die Liste leer ist, gibt es nichts zu ersetzen
    if not zahlen:
        return []

    tail = ersetze_zahl(zahlen[1:], alt, neu)

    if zahlen[0] == alt:
        #zahlen[0] = neu
        return [neu] + tail
    else:
        return [zahlen[0]] + tail



# Teste deinen Code hier:
test_liste = [1, 2, 3, 2, 4, 2]
print(ersetze_zahl(test_liste, alt=2, neu=99))
# Sollte exakt [1, 99, 3, 99, 4, 99] ausgeben
```

### Phase 1: Der Weg nach unten (Aufsplittung)

Die Funktion wird mit der Testliste `[1, 2, 3, 2, 4, 2]` aufgerufen. Da die Liste nicht leer ist, wird sie in jeder Runde in das erste Element (`zahlen[0]`) und den Rest (`zahlen[1:]`) zerlegt:

- **Aufruf 1:** Liste ist `[1, 2, 3, 2, 4, 2]`.
  - Erstes Element = `1`.
  - Der Rest `[2, 3, 2, 4, 2]` wird tiefer geschickt.
- **Aufruf 2:** Liste ist `[2, 3, 2, 4, 2]`.
  - Erstes Element = `2`.
  - Der Rest `[3, 2, 4, 2]` wird tiefer geschickt.
- **Aufruf 3:** Liste ist `[3, 2, 4, 2]`.
  - Erstes Element = `3`.
  - Der Rest `[2, 4, 2]` wird tiefer geschickt.
- **Aufruf 4:** Liste ist `[2, 4, 2]`.
  - Erstes Element = `2`.
  - Der Rest `[4, 2]` wird tiefer geschickt.
- **Aufruf 5:** Liste ist `[4, 2]`.
  - Erstes Element = `4`.
  - Der Rest `[2]` wird tiefer geschickt.
- **Aufruf 6:** Liste ist `[2]`.
  - Erstes Element = `2`.
  - Der Rest `[]` (leer) wird tiefer geschickt.

---

### Phase 2: Der Basis-Fall (Der Wendepunkt)

- **Aufruf 7:** Liste ist `[]`.
  - Die Bedingung `if not zahlen:` ist jetzt wahr.
  - Die Funktion stoppt und gibt `[]` zurück.
  - Ab hier kehrt die Logik um und die Rückgabewerte wandern nach oben.

---

### Phase 3: Der Weg nach oben (Zusammensetzen mit `tail`)

Jetzt wird die Liste von hinten nach vorne wieder zusammengesetzt. Dabei wird geprüft, ob das erste Element eine `2` war. Wenn ja, wird sie durch `99` ersetzt.

- **Zurück in Aufruf 6:** Erstes Element war `2`, `tail` ist `[]`.
  - `2 == alt` → `if`-Zweig: `[99] + []`
  - **Rückgabe:** `[99]`
- **Zurück in Aufruf 5:** Erstes Element war `4`, `tail` ist `[99]`.
  - `4 != alt` → `else`-Zweig: `[4] + [99]`
  - **Rückgabe:** `[4, 99]`
- **Zurück in Aufruf 4:** Erstes Element war `2`, `tail` ist `[4, 99]`.
  - `2 == alt` → `if`-Zweig: `[99] + [4, 99]`
  - **Rückgabe:** `[99, 4, 99]`
- **Zurück in Aufruf 3:** Erstes Element war `3`, `tail` ist `[99, 4, 99]`.
  - `3 != alt` → `else`-Zweig: `[3] + [99, 4, 99]`
  - **Rückgabe:** `[3, 99, 4, 99]`
- **Zurück in Aufruf 2:** Erstes Element war `2`, `tail` ist `[3, 99, 4, 99]`.
  - `2 == alt` → `if`-Zweig: `[99] + [3, 99, 4, 99]`
  - **Rückgabe:** `[99, 3, 99, 4, 99]`
- **Zurück in Aufruf 1 (Startpunkt):** Erstes Element war `1`, `tail` ist `[99, 3, 99, 4, 99]`.
  - `1 != alt` → `else`-Zweig: `[1] + [99, 3, 99, 4, 99]`
  - **Endgültige Rückgabe:** `[1, 99, 3, 99, 4, 99]`

---

### Zusammenfassung

Der Code läuft wie ein Bumerang: Er wirft die Elemente nacheinander ab, bis er am leeren Ende ankommt, und sammelt sie auf dem Rückweg wieder auf. War ein Element die gesuchte Zahl (`alt`), packt er stattdessen die neue Zahl (`neu`) auf den Stapel.

```python
==============================================================================
PHASE 1: DER WEG NACH UNTEN (Aufsplittung)
==============================================================================

Aufruf 1:  [1, 2, 3, 2, 4, 2]  -->  1  abgeschnitten, Rest weitergeben
                                   |
Aufruf 2:     [2, 3, 2, 4, 2]  -->  2  abgeschnitten, Rest weitergeben
                                   |
Aufruf 3:        [3, 2, 4, 2]  -->  3  abgeschnitten, Rest weitergeben
                                   |
Aufruf 4:           [2, 4, 2]  -->  2  abgeschnitten, Rest weitergeben
                                   |
Aufruf 5:              [4, 2]  -->  4  abgeschnitten, Rest weitergeben
                                   |
Aufruf 6:                 [2]  -->  2  abgeschnitten, Rest weitergeben
                                   |
==============================================================================
PHASE 2: DER WENDEPUNKT (Basis-Fall)
==============================================================================

Aufruf 7:                []   -->  Gibt [] zurück! (Bumerang dreht um)
                                   |
==============================================================================
PHASE 3: DER WEG NACH OBEN (Zusammensetzen mit tail)
==============================================================================
                                   |
Aufruf 6:  [99] + []               -->  Rückgabe: [99]   (2 wurde ersetzt)
             ^                             |
Aufruf 5:  [4]  + [99]             -->  Rückgabe: [4, 99]
             ^                             |
Aufruf 4:  [99] + [4, 99]          -->  Rückgabe: [99, 4, 99]   (2 wurde ersetzt)
             ^                             |
Aufruf 3:  [3]  + [99, 4, 99]      -->  Rückgabe: [3, 99, 4, 99]
             ^                             |
Aufruf 2:  [99] + [3, 99, 4, 99]  -->  Rückgabe: [99, 3, 99, 4, 99]   (2 wurde ersetzt)
             ^                             |
Aufruf 1:  [1]  + [99, 3, ... ]    -->  Endgültiges Ergebnis:
                                        [1, 99, 3, 99, 4, 99]
```

```python
def ersetze_zahl_tief(zahlen: list, alt: int, neu: int) -> list:
    """
    Ersetzt rekursiv alle Vorkommen von 'alt' durch 'neu' in einer
    beliebig tief verschachtelten Liste.
    """
    # 1. Basis-Fall: Wenn die Liste leer ist, fertig.
    if not zahlen:
        return []

    # Verarbeite den gesamten Rest der Liste vorab rekursiv
    tail = ersetze_zahl_tief(zahlen[1:], alt, neu)
    head = zahlen[0]

    # 2. Mittelschwerer Zusatz-Fall: Ist das erste Element selbst eine Liste?
    if isinstance(head, list):
        # Wenn ja, tauchen wir REKURSIV in diese Unterliste ab!
        return [ersetze_zahl_tief(head, alt, neu)] + tail

    # 3. Normaler Fall (wie in der leichten Aufgabe)
    if head == alt:
        return [neu] + tail
    else:
        return [head] + tail

# Teste den mittelschweren Code hier:
verschachtelte_liste = [1, [2, 3], [2, [4, 2]]]
print(ersetze_zahl_tief(verschachtelte_liste, alt=2, neu=99))
# Gibt exakt aus: [1, [99, 3], [99, [4, 99]]]
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
