+++
date = '2026-08-23T22:32:00+02:00'
draft = false
title = 'Currying'
showAuthor = false
weight =52
layout = "simple"
summary = "🚀 currying"
+++

```python
from collections.abc import Callable

# Typdefinitionen (für die statische Codeanalyse)
ResizeFunc = Callable[[int, int], tuple[int, int]]
SetMinSizeFunc = Callable[..., ResizeFunc]

# ==========================================
# 1. FUNKTIONSDEFINITION (CURRYING)
# ==========================================

def new_resizer(max_width: int, max_height: int) -> SetMinSizeFunc:
    """
    Schritt 1: Nimmt die maximal erlaubten Dimensionen entgegen.
    Gibt die Funktion 'inner_fct' zurück.
    """
    def inner_fct(min_width: int = 0, min_height: int = 0) -> ResizeFunc:
        """
        Schritt 2: Nimmt die minimal erlaubten Dimensionen entgegen.
        Prüft auf Logikfehler und gibt die Funktion 'innermost_fct' zurück.
        """
        if min_width > max_width or min_height > max_height:
            raise Exception("minimum size cannot exceed maximum size")

        def innermost_fct(width: int, height: int) -> tuple[int, int]:
            """
            Schritt 3: Nimmt die tatsächlichen Bildabmessungen entgegen.
            Grenzt die Werte (Clamping) mithilfe von min() und max() ein.
            """
            # Breite innerhalb der Grenzen halten
            width = min(width, max_width)
            # Falls kleiner als Minimum, wird auf Minimum erhöht
            width = max(width, min_width)

            # Höhe innerhalb der Grenzen halten
            height = min(height, max_height)
            # Falls kleiner als Minimum, wird auf Minimum erhöht
            height = max(height, min_height)

            return width, height

        return innermost_fct

    return inner_fct


# ==========================================
# 2. INTERAKTIVES BEISPIEL FÜR DEINE IDLE
# ==========================================

print("--- Boot.dev Currying Resizer Test ---")

# Wir definieren feste Grenzwerte (Max: 800x600, Min: 200x100)
# Durch das Verschachteln (Currying) "merkt" sich die finale Funktion diese Werte.
set_min_size = new_resizer(800, 600)
resize_image = set_min_size(200, 100)

print("Grenzen gesetzt: Max(800, 600), Min(200, 100)")
print("Gib Testwerte ein, um das Clamping zu sehen.\n")

try:
    # Benutzereingabe abfragen
    user_w = int(input("Gewünschte Bildbreite eingeben: "))
    user_h = int(input("Gewünschte Bildhöhe eingeben: "))

    # Funktion ausführen
    final_width, final_height = resize_image(user_w, user_h)

    # Ergebnis anzeigen
    print("\n--- ERGEBNIS ---")
    print(f"Eingegebene Größe: ({user_w}, {user_h})")
    print(f"Bereinigte Größe:  ({final_width}, {final_height})")

except ValueError:
    print("\n[Fehler] Bitte gib nur ganze Zahlen ein!")

# ==========================================
# 3. BEISPIEL-OUTPUTS FÜR DEIN VERSTÄNDNIS
# ==========================================
"""
BEISPIEL-DURCHLAUF 1 (Bild ist zu groß):
----------------------------------------
Grenzen gesetzt: Max(800, 600), Min(200, 100)
Gewünschte Bildbreite eingeben: 1200
Gewünschte Bildhöhe eingeben: 500

--- ERGEBNIS ---
Eingegebene Größe: (1200, 500)
Bereinigte Größe:  (800, 500)  <-- Breite wurde auf 800 gedrosselt!


BEISPIEL-DURCHLAUF 2 (Bild ist zu klein):
----------------------------------------
Grenzen gesetzt: Max(800, 600), Min(200, 100)
Gewünschte Bildbreite eingeben: 150
Gewünschte Bildhöhe eingeben: 400

--- ERGEBNIS ---
Eingegebene Größe: (150, 400)
Bereinigte Größe:  (200, 400)  <-- Breite wurde auf das Minimum von 200 angehoben!
"""

```

```python
from collections.abc import Callable

def lines_with_sequence(char: str) -> Callable[[int], Callable[[str], int]]:

    def with_char(length: int) -> Callable[[str], int]:
        sequence = char * length

        def with_length(doc):
            split_doc = doc.split("\n")

            # reduce ersetzt hier die for-schleife
            #return reduce(lambda count, line: count + (1 if sequence in line else 0), split_doc, 0)

            count = 0
            for i in split_doc:
                if sequence in i:
                    count += 1
            return count
        return with_length

    return with_char

# Beispiel 1: Suche nach drei Rauten ("###")
doc1 = "###\n@##\n$$$\n###"
ausgabe1 = lines_with_sequence("#")(3)(doc1)

# Beispiel 2: Suche nach zwei "a" ("aa")
doc2 = "aaaa\nbbbb\nccdd\naabb"
ausgabe2 = lines_with_sequence("a")(2)(doc2)

"""
ausgabe1 -> 2
ausgabe2 -> 2
"""
```

```python
def addiere_curried(a):
    def mit_b(b):
        def mit_c(c):
            return a + b + c
        return mit_c
    return mit_b

# Aufruf Schritt für Schritt:
stufe1 = addiere_curried(1)  # Gibt die Funktion 'mit_b' zurück
stufe2 = stufe1(2)           # Gibt die Funktion 'mit_c' zurück
ergebnis = stufe2(3)         # Berechnet das Endergebnis: 6

# Oder kurz in einer Zeile:
ergebnis_kurz = addiere_curried(1)(2)(3) # 6

```

```python
# Wir fixieren das erste Argument (a = 10)
addiere_zehn = addiere_curried(10)

# Jetzt können wir 'addiere_zehn' wie eine eigenständige Funktion nutzen
print(addiere_zehn(5)(2))  # Ausgabe: 17 (10 + 5 + 2)
```

### 🧩 Was ist Currying?

**Currying** ist eine Technik aus der funktionalen Programmierung. Dabei wird eine Funktion, die mehrere Argumente erwartet, in eine Kette von Funktionen zerlegt, die jeweils **nur ein einziges Argument** akzeptieren.

#### Das Prinzip im direkten Vergleich:

```python
# Standard: Alle Argumente auf einmal
def summe(a, b):
    return a + b

print(summe(3, 5)) # 8

# Curried: Ein Argument nach dem anderen (mittels Closures)
def summe_curried(a):
    return lambda b: a + b

print(summe_curried(3)(5)) # 8
```

{{< alert type="info" card="true" >}}
**Nutzen in der Praxis:**  
Currying ermöglicht die sogenannte _Partial Application_ (teilweise Anwendung). Du kannst Funktionen "vorkonfigurieren" und die restlichen Argumente erst später im Code hinzufügen.
{{< /alert >}}
