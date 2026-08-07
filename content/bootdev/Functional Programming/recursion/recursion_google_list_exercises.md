+++
date = '2026-08-06T20:31:50+02:00'
draft = false
title = 'Recursion_google_list_exercises'
showAuthor = false
weight =59
layout = "simple"
summary = "🚀 simple google  exercises on list recursion"
+++

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
