+++
date = '2026-09-28T21:59:15+02:00'
draft = false
title = 'other recursions'
showAuthor = false
weight =70
layout = "simple"
summary = "🚀 othere recursions"
+++

> boot.dev training

```python
def sum_nested_integers(value):
    # 1. BASISTYP-PRÜFUNG: Ist es eine echte Ganzzahl?
    if type(value) == int:
        return value

    # 2. STRUKTUR-PRÜFUNG: Wenn es eine Liste ist, starte die Sequenz-Rekursion
    if isinstance(value, list):
        return sum_sequence(value, 0)

    # 3. STRUKTUR-PRÜFUNG: Wenn es ein Dict ist, nimm nur die Werte und starte die Sequenz-Rekursion
    if isinstance(value, dict):
        return sum_sequence(tuple(value.values()), 0)

    # 4. IGNORIEREN: Strings, Booleans, Floats etc. liefern einfach 0
    return 0


def sum_sequence(values, index):
    # BASISFALL: Wenn der Zeiger am Ende der Liste angekommen ist, stoppe
    if index == len(values):
        return 0

    # REKURSIONSSCHRITT: Berechne das aktuelle Element + den Rest der Liste
    return sum_nested_integers(values[index]) + sum_sequence(values, index + 1)
```

---

title: "Rekursion Meistern: Verschachtelte Listen & Dictionaries in Python addieren"
date: 2026-09-28
draft: false
description: "Eine verständliche Schritt-für-Schritt-Anleitung zur wechselseitigen Rekursion in Python – ohne Schleifen und ohne Mutationen."
tags: ["python", "rekursion", "algorithmen", "bootdev"]
categories: ["Programming"]
showToc: true

---

Rekursion fühlt sich am Anfang oft wie Magie an. Wenn man dann noch gezwungen ist, **keine Schleifen** zu benutzen und tief verschachtelte Datenstrukturen zu durchsuchen, raucht schnell der Kopf.

In dieser Anleitung zerlegen wir eine elegante Lösung für eine klassische Coding-Challenge: Das rekursive Aufsummieren aller Integer in beliebig tiefen Listen und Dictionaries.

---

## Die Problemstellung

Wir wollen eine Funktion `sum_nested_integers(value)` schreiben, die:

1. **Alle Ganzzahlen (Integers)** in verschachtelten Listen und Dictionary-Werten findet und addiert.
2. **Booleans, Strings und andere Datentypen** komplett ignoriert.
3. **Keine `for`- oder `while`-Schleifen** verwendet.
4. Den ursprünglichen Input **nicht verändert (keine Mutation)**.

### Beispiel-Input & Output

```python
data = [4, {"cargo": [3, True, "2"]}, -1]
# Ergebnis sollte sein: 4 + 3 + (-1) = 6
# (True und "2" werden ignoriert)
```

---

## Der Code

Die Lösung nutzt **Wechselseitige Rekursion** (Mutual Recursion). Zwei Funktionen teilen sich die Arbeit und spielen sich gegenseitig die Bälle zu:

```python
def sum_nested_integers(value):
    # 1. BASISTYP-PRÜFUNG: Ist es eine echte Ganzzahl?
    if type(value) == int:
        return value

    # 2. STRUKTUR-PRÜFUNG: Wenn es eine Liste ist, starte die Sequenz-Rekursion
    if isinstance(value, list):
        return sum_sequence(value, 0)

    # 3. STRUKTUR-PRÜFUNG: Wenn es ein Dict ist, extrahiere die Werte
    if isinstance(value, dict):
        return sum_sequence(tuple(value.values()), 0)

    # 4. IGNORIEREN: Strings, Booleans, Floats etc. liefern einfach 0
    return 0


def sum_sequence(values, index):
    # BASISFALL: Wenn der Zeiger am Ende der Liste ist, stoppe die Rekursion
    if index == len(values):
        return 0

    # REKURSIONSSCHRITT: Berechne aktuelles Element + den Rest der Liste
    return sum_nested_integers(values[index]) + sum_sequence(values, index + 1)
```

---

## Die Erklärung: Schritt für Schritt

Das Geheimnis dieser Lösung liegt im Zusammenspiel der beiden Funktionen wie bei einem Tischtennisspiel.

### 1. Die Einzelelement-Prüfung: `sum_nested_integers`

Diese Funktion kümmert sich immer nur um **ein einziges Objekt** zurzeit. Sie stellt die Frage: _„Was bist du?“_

- **Die Boolean-Falle (`type(value) == int`):**
  In Python ist der Typ `bool` eine Unterklasse von `int`. Würden wir `isinstance(value, int)` nutzen, würde Python bei `True` denken: _"Ah, eine 1!"_ und sie fälschlicherweise mitzählen. `type(value) == int` ist strenger und lässt Booleans durchfallen.
- **Die Struktur-Erkennung:**
  Trifft die Funktion auf eine Liste oder ein Dictionary, weiß sie, dass sie das nicht alleine lösen kann. Sie wirft die Daten rüber zu `sum_sequence`. Bei Dictionaries nutzt sie `.values()`, da uns die Keys (die Beschriftungen) egal sind, und verpackt sie in ein `tuple`, damit man sie per Index ansteuern kann.

### 2. Der Schleifen-Ersatz: `sum_sequence`

Da Schleifen verboten sind, übernimmt diese Funktion die Iteration mittels **linearer Rekursion**. Sie bekommt eine flache Sequenz und einen Zeiger (`index`).

- **Der Stopp-Befehl (`index == len(values)`):**
  Sobald der Zeiger das Ende der Liste erreicht hat, gibt die Funktion `0` zurück. Die Rekursion bricht ab und wandert den Pfad zurück.
- **Das Teilen und Herrschen:**
  Die Zeile `return sum_nested_integers(values[index]) + sum_sequence(values, index + 1)` ist das Herzstück:
  1. Sie nimmt das Element am aktuellen `index` und schickt es zurück an `sum_nested_integers` (falls es wieder eine verschachtelte Liste ist, gräbt sich das Programm hier tiefer).
  2. Sie ruft sich selbst mit `index + 1` auf, um den Rest der Liste abzuarbeiten.
  3. Am Ende addiert sie beide Ergebnisse zusammen.

---

## Visueller Ablauf (Ablaufprotokoll)

Schauen wir uns an, was im Speicher passiert, wenn wir `[4, [3]]` übergeben:

1. `sum_nested_integers([4, [3]])` sieht eine Liste → ruft `sum_sequence([4, [3]], 0)` auf.
2. `sum_sequence` (Index 0) nimmt die `4` → `sum_nested_integers(4)` gibt **`4`** zurück.
3. `sum_sequence` wandert weiter zu Index 1: Das ist die Liste `[3]`.
4. `sum_nested_integers([3])` sieht eine Liste → ruft `sum_sequence([3], 0)` auf.
5. Diese innere `sum_sequence` (Index 0) nimmt die `3` → `sum_nested_integers(3)` gibt **`3`** zurück.
6. Die innere `sum_sequence` wandert zu Index 1 → Ende erreicht, gibt **`0`** zurück.
7. Die innere Liste wird aufgelöst: `3 + 0 = 3`.
8. Die äußere `sum_sequence` rechnet alles zusammen: `4 + 3 = 7`.

## Fazit

Wechselseitige Rekursion ist ein mächtiges Werkzeug, um komplexe, baumartige Strukturen zu durchlaufen. Indem wir die Typenprüfung von der Sequenz-Iteration trennen, bleibt der Code modular, extrem sauber und hält alle funktionalen Programmierbedingungen ein.
