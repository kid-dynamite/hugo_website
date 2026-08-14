+++
date = '2026-08-14T01:20:44+02:00'
draft = false
title = 'OOP_google_cheat-sheet'
showAuthor = false
weight =22
layout = "simple"
summary = "🚀 OOP google cheat-sheet"
+++

---

title: "Das inoffizielle PCAP-Zertifizierungsbuch: Was dein Lehrbuch verschweigt"
date: 2026-08-14
description: "Ein unzensierter Überlebensguide für die PCAP-Python-Prüfung. Die wichtigsten OOP-Stolperfallen (MRO, Name Mangling, Hashing), die in Standard-Lehrbüchern fehlen."
tags: ["Python", "PCAP", "Zertifizierung", "OOP", "Programmierung"]
categories: ["Programming"]
series: ["Python Zertifizierung"]
draft: false

---

Dieses Handbuch ist die perfekte Ergänzung zu deinen offiziellen PCAP-Unterlagen (Certified Associate in Python Programming). Während Lehrbücher oft nur die saubere Theorie erklären, zeigt dieser Guide die fiesen Grenzfälle des CPython-Interpreters, die in der echten Prüfung abgefragt werden.

---

## 📌 Kapitel 1: Der wahre Lebenszyklus eines Objekts (`__new__` vs. `__init__`)

**Was das Lehrbuch sagt:** _„`__init__` ist der Konstruktor und erstellt das Objekt.“_ (**Falsch!**)  
**Was die Prüfung verlangt:** Du musst wissen, dass die Objekterstellung in Python zweistufig abläuft.

### Das fehlende Wissen

- **`__new__(cls)`**: Das ist der **echte** Konstruktor. Diese Methode wird _zuerst_ aufgerufen. Sie fordert den Speicherplatz vom Betriebssystem an, erschafft die Instanz und gibt sie zurück.
- **`__init__(self)`**: Das ist lediglich der _Initialisierer_. Er wird _danach_ aufgerufen, bekommt das fertige Objekt übergeben (`self`) und befüllt es mit Attributen. Diese Methode darf **niemals** etwas anderes als `None` zurückgeben.

### Der Prüfungs-Klassiker

In der Prüfung siehst du oft Code, bei dem beide Methoden Text ausgeben. Du musst die Reihenfolge im Schlaf kennen:

```python
class Exam:
    def __new__(cls):
        print("1. New")
        return super().__new__(cls) # Erzeugt das eigentliche Objekt im Speicher

    def __init__(self):
        print("2. Init")

obj = Exam()
```

- **Ausgabe:** Immer `1. New` gefolgt von `2. Init`.
- **🚨 Die Falle:** Wenn in `__init__` ein `return 5` steht, wirft Python sofort einen `TypeError`. `__init__` darf keinen Wert zurückgeben!

---

## 📌 Kapitel 2: Das Namespace-Dilemma (Klassen- vs. Instanzvariablen)

**Was das Lehrbuch sagt:** _„Klassenvariablen werden von allen Instanzen geteilt. Ändert man sie, ändert sie sich für alle.“_ (**Stimmt nur halb!**)  
**Was die Prüfung verlangt:** Du musst verstehen, wie Python Variablen in den Namespaces sucht (Lookup-Reihenfolge).

### Das fehlende Wissen

Python sucht Variablen immer von innen nach außen: **Erst im Namespace des spezifischen Objekts, dann im Namespace der Klasse.**  
Wenn du `objekt.variable = wert` schreibst, änderst du _nicht_ die Klassenvariable. Du erschaffst stattdessen heimlich eine komplett neue **Instanzvariable** im Speicher dieses einen Objekts.

### Der Prüfungs-Klassiker

```python
class Counter:
    count = 10 # Klassenvariable

c1 = Counter()
c2 = Counter()

c1.count = 20      # FALLE: Erstellt eine Instanzvariable NUR für c1!
Counter.count = 30 # Ändert die globale Klassenvariable für alle anderen

print(c1.count, c2.count)
```

- **Ausgabe:** `20 30`
- **Warum?** `c1` hat durch die Zuweisung sein eigenes `count` im Objekt-Namespace bekommen (`20`). `c2` besitzt kein eigenes `count`, sucht deshalb eine Ebene höher in der Klasse und findet dort die aktualisierte `30`.

---

## 📌 Kapitel 3: Das unzensierte `super()` (MRO-Reihenfolge)

**Was das Lehrbuch sagt:** _„Mit `super()` rufst du die Elternklasse auf.“_ (**Bei Mehrfachvererbung komplett falsch!**)  
**Was die Prüfung verlangt:** Das fehlerfreie Auflösen der **MRO** (Method Resolution Order).

### Das fehlende Wissen

`super()` bedeutet nicht „gehe zu meinen Eltern“, sondern: **„Gehe zum nächsten Eintrag in der MRO-Liste des aktuellen Objekts.“** Python berechnet bei Mehrfachvererbung nach dem C3-Linearisierungs-Algorithmus eine strikte Suchreihenfolge von links nach rechts.

### Der Prüfungs-Klassiker (Das Diamant-Problem)

```python
class A:
    def go(self): print("A")

class B(A):
    def go(self): print("B"); super().go()

class C(A):
    def go(self): print("C"); super().go()

class D(B, C):
    def go(self): print("D"); super().go()

D().go()
```

- **Ausgabe:** `D -> B -> C -> A`
- **Die Überraschung:** Warum ruft `B` plötzlich `C` auf, obwohl `C` überhaupt keine Elternklasse von `B` ist? Weil die MRO von Klasse `D` so aussieht: `[D, B, C, A, object]`. Wenn `B` via `super()` nach dem nächsten Eintrag in der Kette fragt, liefert Python eiskalt `C` zurück!
- **🚨 Die K.-o.-Falle:** Klassen wie `class X(A, B): pass` crashen in Python schon beim Laden des Skripts mit einem `TypeError (Cannot create a consistent MRO)`, falls `B` bereits von `A` erbt. Eine Unterklasse muss in den Klammern _immer vor_ ihrer eigenen Basisklasse deklariert werden.

---

## 📌 Kapitel 4: Das Hashing-Verbot (`__eq__` zerstört dein Set)

**Was das Lehrbuch sagt:** _„Mit `__eq__` kannst du definieren, wann zwei Objekte als logisch gleich gelten.“_ (Mehr steht dazu meist nicht drin.)  
**Was die Prüfung verlangt:** Zu wissen, unter welchen Bedingungen ein Objekt überhaupt in Dictionaries genutzt werden darf.

### Das fehlende Wissen

Damit ein Objekt ein Schlüssel (Key) in einem `dict` oder ein Element in einem `set` sein darf, muss es **hashbar** (unveränderlich) sein. Standardmäßig ist das jedes benutzerdefinierte Objekt in Python. Aber: **Sobald du die Methode `__eq__` selbst programmierst, entzieht Python dem Objekt automatisch die Hashing-Fähigkeit!**

### Der Prüfungs-Klassiker

```python
class User:
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name

u = User("Anna")
daten = {u: "Admin"} # Code versucht, das Objekt als Key in ein Dict zu packen
```

- **Ausgabe:** `TypeError: unhashable type: 'User'`
- **Warum?** Python argumentiert logisch: _„Wenn der Programmierer Gleichheit definiert, ändern sich im Verlauf bestimmt die Attribute des Objekts. Wenn sich Attribute ändern, würde sich der Hash-Wert im Dict-Index verschieben und das Dict korrumpieren. Also verbiete ich das Hashing lieber ganz!“_
- **Die Lösung:** Das Objekt wird für Sets/Dicts unbrauchbar, es sei denn, man definiert zusätzlich die Methode `__hash__` manuell mit.

---

## 📌 Kapitel 5: Die String-Identitätskrise (`__str__` vs. `__repr__`)

**Was das Lehrbuch sagt:** _„`__str__` macht dein Objekt über einen print-Befehl druckbar.“_  
**Was die Prüfung verlangt:** Den fundamentalen Unterschied zu `__repr__` zu kennen, wenn Objekte in Kollektionen stecken.

### Das fehlende Wissen

- **`__str__`**: Gedacht für den Endnutzer (schön, sauber und lesbar). Wird aufgerufen bei `print(obj)` oder `str(obj)`.
- **`__repr__`**: Gedacht für den Entwickler (eindeutig, oft identisch mit dem Code zur Objekterzeugung). Wird aufgerufen, wenn das Objekt **innerhalb einer Liste, eines Tuples oder eines Dictionaries** ausgegeben wird.

### Der Prüfungs-Klassiker

```python
class Card:
    def __str__(self): return "Schön"
    def __repr__(self): return "Entwickler"

c = Card()
print(c)        # Fall A: Direktes Drucken
print([c, c])   # Fall B: Drucken innerhalb einer Liste
```

- **Ausgabe Fall A:** `Schön` (da direkt vom String-Kontext aufgerufen).
- **Ausgabe Fall B:** `['Entwickler', 'Entwickler']` (da Kollektionen für ihre Elemente immer nach der `__repr__`-Darstellung verlangen!).
- **Wichtige Prüfungsregel:** Wenn `__str__` in einer Klasse fehlt, springt Python automatisch auf `__repr__` als Backup an. Fehlt auch `__repr__`, erhältst du nur die Standard-Speicheradresse (`<__main__.Card object at 0x... >`).

---

## 🛡️ Das richtige Mindset für die PCAP-Prüfung

Wenn du in der Prüfung Multiple-Choice-Antworten siehst, die Begriffe wie `TypeError: unhashable type` oder `Inconsistent MRO` enthalten, gerate nicht in Panik. Das Python Institute nutzt diese Begriffe extrem oft als falsche Antwortmöglichkeiten (**Distraktoren**), um Prüflinge zu verunsichern, die nur raten.

Da du nun genau weißt, welche realen Mechanismen dahinterstecken, kannst du diese Optionen in normalen OOP-Fragen gezielt und logisch ausschließen!
