+++
date = '2026-08-23T22:16:52+02:00'
draft = false
title = 'Closures'
showAuthor = false
weight =51
layout = "simple"
summary = "🚀 closures"
+++

```python
def erstelle_zaehler():
    anzahl = 0  # Diese Variable "merkt" sich die Closure

    def zaehler():
        nonlocal anzahl  # Erlaubt es, die äußere Variable zu ändern
        anzahl += 1
        return anzahl

    return zaehler  # Wir geben die Funktion selbst zurück, ohne sie aufzurufen

# Anwendung:
mein_zaehler = erstelle_zaehler()

print(mein_zaehler())  # Ausgabe: 1
print(mein_zaehler())  # Ausgabe: 2

```

```python
def multipliziere_mit(faktor):
    def funktion(zahl):
        return zahl * faktor
    return funktion

verdoppler = multipliziere_mit(2)
verdreifacher = multipliziere_mit(3)

print(verdoppler(5))     # Ausgabe: 10 (5 * 2)
print(verdreifacher(5))  # Ausgabe: 15 (5 * 3)
```

```python
def erstelle_spieler(name, start_hp):
    # Diese Variablen sind von außen geschützt (Kapselung)
    spieler_name = name
    hp = start_hp

    # Die innere Funktion (die Closure)
    def schaden_zufuegen(menge):
        nonlocal hp  # Erlaubt das Ändern der Variable im äußeren Scope
        hp -= menge
        if hp <= 0:
            return f"{spieler_name} wurde besiegt!"
        return f"{spieler_name} hat noch {hp} HP übrig."

    # Wir geben die innere Funktion zurück
    return schaden_zufuegen

# --- Anwendung ---

# Wir erstellen einen konkreten Spieler
held_angreifen = erstelle_spieler("Arthur", 100)

# Jetzt fügen wir Schaden zu
print(held_angreifen(30))  # Ausgabe: Arthur hat noch 70 HP übrig.
print(held_angreifen(50))  # Ausgabe: Arthur hat noch 20 HP übrig.
print(held_angreifen(25))  # Ausgabe: Arthur wurde besiegt!
```

### 1. Die Typen-Signatur lesen

Schau dir den Rückgabetyp genau an:

```python
-> Callable[[str, str, str], Styles]
```

Das bedeutet übersetzt:

- **Äußere Funktion:** Deine Funktion `css_styles` muss eine _innere Funktion_ zurückgeben.
- **Innere Funktion:** Diese innere Funktion muss genau **drei Argumente** entgegennehmen (alle vom Typ `str`).
- **Rückgabewert:** Am Ende muss diese innere Funktion ein Dictionary vom Typ `Styles` zurückgeben.

---

### 2. Das `copy`-Modul nutzen

Oben im Code steht nicht ohne Grund `import copy`.

{{< alert type="warning" card="true" >}}
**Achtung bei Mutationen!**  
In der funktionalen Programmierung wollen wir Seiteneffekte und Mutationen vermeiden (_Pure Functions_). Wenn du das originale `initial_styles` direkt veränderst, manipulierst du den Zustand außerhalb der Closure.
{{< /alert >}}

Nutze in deiner inneren Funktion `copy.deepcopy()`, um eine echte Kopie von `initial_styles` (oder dem aktuellen Zustand) zu erstellen, bevor du Einträge hinzufügst oder änderst.

---

### 3. Brauchst du `nonlocal`?

Das hängt ganz von deinem gewählten Ansatz ab:

- **Variante A:** Wenn du den Zustand von `initial_styles` bei jedem Aufruf dauerhaft in der Closure verändern und speichern willst, hilft dir eine Kopie und eventuell das Keyword `nonlocal`.
- **Variante B:** Wenn die Funktion einfach nur auf Basis der `initial_styles` eine neue, erweiterte Kopie zurückgeben soll, reicht der normale Lesezugriff auf die äußere Variable völlig aus.

---

### 🚀 Dein Fahrplan im Code

Folge diesen Schritten, um die Aufgabe Schritt für Schritt zu lösen:

1. **Innere Funktion definieren:** Erstelle innerhalb von `css_styles` eine neue Funktion mit 3 Parametern.
2. **Kopieren:** Kopiere die Styles innerhalb dieser inneren Funktion mit `copy.deepcopy()`.
3. **Modifizieren:** Bearbeite die Kopie anhand der 3 Parameter (wahrscheinlich ein CSS-Selector, eine Property und ein Value).
4. **Zurückgeben (Innen):** Gib die modifizierte Kopie am Ende der inneren Funktion zurück.
5. **Zurückgeben (Außen):** Gib ganz unten in der äußeren Funktion die innere Funktion selbst zurück (`return name_der_inneren_funktion`).
   > Example

```python
from collections.abc import Callable


def multiply(x: int, y: int) -> int:
    return x * y


def add(x: int, y: int) -> int:
    return x + y


# self_math is a higher-order function
# input: a function that takes two arguments and returns a value
# output: a new function that takes one argument and returns a value
def self_math(math_func: Callable[[int, int], int]) -> Callable[[int], int]:
    def inner_func(x: int) -> int:
        return math_func(x, x)

    return inner_func


square_func: Callable[[int], int] = self_math(multiply)
double_func: Callable[[int], int] = self_math(add)

print(square_func(5))
# prints 25

print(double_func(5))
# prints 10
```

---

title: "Python Closures einfach erklärt"
date: 2026-08-26
draft: false
description: "Eine Schritt-für-Schritt-Erklärung von Closures in Python mit Code-Beispielen und Terminal-Inspektion."
tags: ["python", "programming", "closures"]
categories: ["Tutorials"]
showAuthor: true
showTableOfContents: true

---

{{< lead >}}
Ein **Closure (Verschluss)** entsteht in Python immer dann, wenn eine innere Funktion auf Variablen oder Funktionen der äußeren Funktion zugreift, selbst nachdem die äußere Funktion bereits fertig ausgeführt wurde.
{{< /lead >}}

Hier ist die genaue Schritt-für-Schritt-Erklärung, wie dieser Code funktioniert:

## Die Schritt-für-Schritt-Erklärung

### Schritt 1: Die Werkstatt (`self_math`) verstehen

`self_math` ist wie eine Fabrik, die neue mathematische Funktionen baut.

- Sie erwartet als Input eine Funktion mit zwei Argumenten (wie `multiply` oder `add`).
- Sie gibt eine brandneue Funktion (`inner_func`) zurück, die nur noch _ein_ Argument erwartet.

### Schritt 2: Der Moment, in dem das Closure entsteht

Schauen wir uns diese Zeile an:

```python
square_func = self_math(multiply)
```

1. Du rufst `self_math` auf und übergibst `multiply` als `math_func`.
2. `self_math` geht im Speicher auf. Für diesen Aufruf ist `math_func = multiply`.
3. Drinnen wird `inner_func` definiert. Diese Funktion merkt sich: _"Ich muss später `math_func` aufrufen."_
4. `self_math` gibt diese `inner_func` zurück und **wird beendet**.

> **Das Closure-Wunder:** Obwohl `self_math` jetzt fertig und "tot" ist, hat sich `inner_func` die Variable `math_func` (also `multiply`) in einem unsichtbaren Rucksack eingepackt. Sie ist darin "eingeschlossen" (_closed over_). Dieser Rucksack ist das Closure.

### Schritt 3: Die Anwendung des Closures

Jetzt rufst du die neu gebaute Funktion auf:

```python
print(square_func(5))
```

1. `square_func` ist eigentlich die `inner_func`, die du vorhin gespeichert hast.
2. Du übergibst ihr die `5`. Also ist `x = 5`.
3. Code in der Funktion: `return math_func(x, x)`
4. Sie schaut in ihren Rucksack, findet dort `multiply` und führt aus: `multiply(5, 5)`.
5. Das Ergebnis ist `25`.

### Schritt 4: Ein zweites, unabhängiges Closure

In der nächsten Zeile passiert genau dasselbe, nur mit einer anderen Zutat:

```python
double_func = self_math(add)
```

Jetzt baut `self_math` eine neue `inner_func`. In deren Rucksack wird diesmal `add` hineingepackt. Wenn du `double_func(5)` aufrufst, holt sie `add` heraus und rechnet `add(5, 5)`, was `10` ergibt.

---

## Zusammenfassung: Warum ist es ein Closure?

- **Verschachtelung:** Es gibt eine innere Funktion (`inner_func` in `self_math`).
- **Variablen-Zugriff:** Die innere Funktion nutzt eine Variable der äußeren Funktion (`math_func`).
- **Überleben:** Die innere Funktion "überlebt" den Aufruf der äußeren Funktion (`square_func` und `double_func` können später im Code genutzt werden).

---

## Closures direkt im Terminal anzeigen lassen

In Python kannst du diesen unsichtbaren Rucksack über das versteckte Attribut `__closure__` direkt im Terminal ausspionieren.

Kopiere einfach diesen Code in dein Terminal, um es selbst zu sehen:

```python
from collections.abc import Callable

def multiply(x: int, y: int) -> int: return x * y
def add(x: int, y: int) -> int: return x + y

def self_math(math_func: Callable[[int, int], int]) -> Callable[[int], int]:
    def inner_func(x: int) -> int:
        return math_func(x, x)
    return inner_func

square_func = self_math(multiply)
double_func = self_math(add)

# --- Ab hier wird spioniert ---

# 1. Prüfen, ob ein Closure existiert
print(square_func.__closure__)
# Output: (<cell at 0x...: function object at 0x...>,)

# 2. Den Inhalt des Rucksacks auspacken
print(square_func.__closure__[0].cell_contents)
# Output: <function multiply at 0x...>

# 3. Zum Vergleich die andere Funktion prüfen
print(double_func.__closure__[0].cell_contents)
# Output: <function add at 0x...>
```

### Was passiert hier genau?

- **`__closure__`**: Ein Tupel aus Speicherzellen (sogenannten `cell`-Objekten). Wenn eine Funktion kein Closure ist, ist dieses Attribut einfach `None`.
- **`cell_contents`**: Damit holst du den echten Wert aus der Speicherzelle heraus. Python hält diese Variable im Hintergrund am Leben, solange `square_func` existiert.

> Boot.dev exercise

```python
# 1. Die Funktions-Fabrik
def doc_format_checker_and_converter(conversion_function, valid_formats):

    # Die innere Funktion merkt sich die Parameter von oben
    def closure_fct(filename, content):
        split_filename = filename.split(".")
        extension = split_filename[-1]

        if extension in valid_formats:
            # Hier wird der "Werkzeug-Funktion" der Text übergeben
            return conversion_function(content)
        else:
            raise ValueError("invalid file format")

    # Wir geben die fertige, maßgeschneiderte Funktion zurück
    return closure_fct


# 2. Die Werkzeuge (Einfache Funktionen)
def capitalize_content(content):
    return content.upper()

def reverse_content(content):
    return content[::-1]


# ==========================================
# ANWENDUNG IM CODE:
# ==========================================

# Wir bauen uns eine Funktion "schrei_konverter"
# conversion_function = capitalize_content
# valid_formats       = ["txt", "md"]
schrei_konverter = doc_format_checker_and_converter(capitalize_content, ["txt", "md"])

# Jetzt benutzen wir die frisch gebaute Funktion wie jede andere Funktion auch:
print(schrei_konverter("datei.txt", "ich lerne closures!"))
# Ausgabe: ICH LERNE CLOSURES!

# Hier schlägt es fehl, weil "pdf" nicht in der erlaubten Liste steht:
try:
    schrei_konverter("dokument.pdf", "wichtiger text")
except ValueError as e:
    print(f"Fehler abgefangen: {e}")
    # Ausgabe: Fehler abgefangen: invalid file format
```

> Formatter

```python
from collections.abc import Callable


def formatter(pattern: str) -> Callable[[str], str]:
    def inner_func(text: str) -> str:
        result: str = ""
        i: int = 0
        while i < len(pattern):
            if pattern[i : i + 2] == "{}":
                result += text
                i += 2
            else:
                result += pattern[i]
                i += 1
        return result

    return inner_func



bold_formatter: Callable[[str], str] = formatter("**{}**")
italic_formatter: Callable[[str], str] = formatter("*{}*")
bullet_point_formatter: Callable[[str], str] = formatter("* {}")


print(bold_formatter("Hello"))
# **Hello**
print(italic_formatter("Hello"))
# *Hello*
print(bullet_point_formatter("Hello"))
# * Hello
```

> nonlocal

```python
from collections.abc import Callable


def concatter() -> Callable[[str], str]:
    doc: str = ""

    def doc_builder(word: str) -> str:
        # "nonlocal" tells Python to use the 'doc'
        # variable from the enclosing scope
        nonlocal doc
        if doc:
            doc += " "
        doc += word
        return doc

    return doc_builder


# save the returned 'doc_builder' function
# to the new function 'harry_potter_aggregator'
harry_potter_aggregator: Callable[[str], str] = concatter()
harry_potter_aggregator("Mr.")
harry_potter_aggregator("and")
harry_potter_aggregator("Mrs.")
harry_potter_aggregator("Dursley")
harry_potter_aggregator("of")
harry_potter_aggregator("number")
harry_potter_aggregator("four,")
harry_potter_aggregator("Privet")

print(harry_potter_aggregator("Drive"))
# Mr. and Mrs. Dursley of number four, Privet Drive
```
