+++
date = '2026-08-13T15:07:26+02:00'
draft = false
title = 'OOP_concepts_1'
showAuthor = false
weight =20
layout = "simple"
summary = "🚀 OOP concepts 1"
+++

> investigating classes --> getattr & setattr

```python
class MyClass:
    pass

obj = MyClass()
obj.a = 1
obj.b = 2
obj.i = 3
obj.ireal = 3.5
obj.z = 5

def incIntsI(obj):
    for name in obj.__dict__.keys():
        if name.startswith("i"):
            val = getattr(obj, name)
            if isinstance(val, int):
                setattr(obj, name, val +1)
                #obj.__dict__[name] = value + 1

print(obj.__dict__)
incIntsI(obj)
print(obj.__dict__)


"""
{'a': 1, 'b': 2, 'i': 3, 'ireal': 3.5, 'z': 5}
{'a': 1, 'b': 2, 'i': 4, 'ireal': 3.5, 'z': 5}
"""

"""
class Player:
    def __init__(self, score):
        self.score = score

p = Player(10)
setattr(p, "score", getattr(p, "score") + 5)
print(p.score)
"""

"""
Frage: Welche der folgenden Zeilen ist äquivalent zu obj.i = obj.i + 1
Lösung:     1) setattr(obj, "i", getattr(obj, "i") + 1)
            2) obj.__dict__["i"] = obj.__dict__["i"] + 1
```

> hasattr

```python
class ExampleClass:
    a = 1
    def __init__(self):
        self.b = 2

example_object = ExampleClass()

print(hasattr(example_object, "b"))
print(hasattr(example_object, "a"))
print(hasattr(ExampleClass, "a"))
print(hasattr(ExampleClass, "b"))

print(example_object.__dict__)


"""
True
True
True
False
{'b': 2}
"""
```

> methods

```python
class Classy:
    varia = 1
    def __init__(self):
        self.var = 2

    def method(self):
        pass

    def __hidden(self):
        pass

obj = Classy()

print(obj.__dict__)
print(Classy.__dict__)

"""
__dict__ ist das interne Wörterbuch, das die Attribute von Objekten und Klassen speichert.
Instanzvariablen (self.x) stecken im __dict__ des Objekts.Klassenvariablen und Methoden
stecken im __dict__ der Klasse.Private Attribute (__double_underscore) werden per
Name Mangling umbenannt, um sie zu schützen.
"""

"""
{'var': 2}
{'__module__': '__main__', 'varia': 1, '__init__': <function Classy.__init__ at 0x00000243063087C0>, 'method': <function Classy.method at 0x000002430637B240>, '_Classy__hidden': <function Classy.__hidden at 0x000002430637BB00>, '__dict__': <attribute '__dict__' of 'Classy' objects>, '__weakref__': <attribute '__weakref__' of 'Classy' objects>, '__doc__': None}
"""
```

> module_bases

```python
class Classy:
    pass

print(Classy.__module__)
obj = Classy()
print(obj.__module__)

print("*********************************")

class SuperOne:
    pass

class SuperTwo:
    pass

class Sub(SuperOne, SuperTwo):
    pass

def printBases(cls):
    print('( ', end='')

    for x in cls.__bases__:
        print(x.__name__, end=' ')

    print(')')

printBases(SuperOne)
printBases(SuperTwo)
printBases(Sub)

"""
print(SuperOne.__bases__)
# Ausgabe: (<class 'object'>,)
"""

"""
__main__
__main__
*********************************
( object )
( object )
( SuperOne SuperTwo )
"""

"""
================================================================================
PCAP-ZERTIFIZIERUNG: ÜBERSICHT KLASSEN-INTROSPEKTION
================================================================================

| Attribut     | Typ    | Existiert bei Klasse? | Existiert bei Instanz? | Beschreibung                                |
| :----------- | :----- | :-------------------: | :--------------------: | :------------------------------------------ |
| __name__     | String | Ja                    | Nein ❌                 | Liefert den Namen der Klasse als Text.     |
| __module__   | String | Ja                    | Ja                     | Liefert den Modulnamen (oft "__main__").   |
| __bases__    | Tupel  | Ja                    | Nein ❌                 | Liefert die direkten Elternklassen.         |

⚠️ WICHTIGE PCAP-FALLEN FÜR DIESE ATTRIBUTE:
1. `obj.__name__` und `obj.__bases__` werfen sofort einen AttributeError!
2. `Klasse.__bases__` liefert ein TUPEL. Für den Namen der ersten Elternklasse
   musst du das Tupel erst per Index auslesen: `Klasse.__bases__[0].__name__`
================================================================================
"""


```

# PCAP-Spickzettel: Warum ist `__bases__` ein Tupel?

Das Attribut `__bases__` existiert **nur auf Klassen** und liefert im Hintergrund **immer ein Tupel** (eine unveränderliche Liste). Das ist so, weil eine Klasse in Python von mehreren Klassen gleichzeitig erben kann (Mehrfachvererbung).

---

### 1. Code-Beispiel (Mehrfachvererbung)

```python
class Papa:
    pass

class Mama:
    pass

class Kind(Papa, Mama):  # Erbt von zwei Klassen
    pass
```

---

### 2. Was passiert im Hintergrund bei `__bases__`?

Wenn du `Kind.__bases__` aufrufst, baut Python unsichtbar dieses Tupel:

```text
Tupel-Inhalt:  ( <class 'Papa'> , <class 'Mama'> )
Index:                 [0]              [1]
```

- **Index [0]** (Der 1. Wert) ist die Klasse `Papa`, weil sie zuerst in der Klammer stand.
- **Index [1]** (Der 2. Wert) ist die Klasse `Mama`, weil sie als zweites dort stand.

---

### 3. Die PCAP-Syntax-Falle (Warum brauche ich `[0]`?)

#### ❌ Der Fehler-Code (Absturz)

```python
print(Kind.__bases__.__name__)
```

- **Warum?** Python sucht `__name__` direkt auf dem Tupel-Container. Ein Tupel hat aber keinen Klassennamen! ➔ **AttributeError**.

#### Der richtige Code

```python
print(Kind.__bases__[0].__name__)
```

- **Warum?**
  1. `Kind.__bases__` holt das Tupel: `(Papa, Mama)`
  2. `[0]` packt das erste Element **aus** dem Tupel aus: die Klasse `Papa`.
  3. `.__name__` holt jetzt den Text-Namen der Klasse `Papa`.
  - **Ausgabe:** `Papa`

---

### 4. Sonderfall: Einfache Vererbung

```python
class EinzelKind(Papa):
    pass
```

Aufruf von `EinzelKind.__bases__` liefert:

```python
( <class 'Papa'> , )
```

- Das Komma am Ende zeigt Python nur, dass es ein Tupel mit einem einzigen Element ist.
- Auch hier liegt die Klasse `Papa` auf **Index [0]**.
- Auch hier **musst** du `[0]` nutzen, um an den Namen zu kommen.

> **str** & **repr**

# PCAP-Spickzettel: `__str__` vs. `__repr__` (String-Repräsentation)

In Python gibt es zwei spezielle Methoden ("Dunder-Methoden"), um ein Objekt in Text umzuwandeln. Die PCAP-Prüfung testet extrem genau, wann Python welche Methode automatisch auswählt.

---

### 1. Die goldenen Regeln für die Prüfung

- **`__str__()`** ➔ Für **Endnutzer** gedacht. Wird aufgerufen bei `print(obj)` oder `str(obj)`.
- **`__repr__()`** ➔ Für **Entwickler** gedacht (Debugging). Wird aufgerufen bei `repr(obj)` oder wenn Objekte **in einer Liste `[]`** stecken.

#### Das Fallback-Prinzip (Der Rettungsanker)

Wenn Python nach `__str__` sucht, es aber in der Klasse nicht findet, stürzt das Programm nicht ab. Es nutzt stattdessen automatisch **`__repr__`** als Ersatz.

_(Andersherum funktioniert das nicht: Wenn `__repr__` gesucht wird, schaut Python niemals in `__str__` nach!)_

---

### 2. Auflösung des Quiz-Beispiels

Hier ist noch einmal der Code aus dem Quiz:

```python
class Star:
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return f"R:{self.name}"

    def __str__(self):
        return f"S:{self.name}"

class Planet(Star):
    def __str__(self):
        return f"P:{self.name}"  # Überschreibt __str__, erbt aber __repr__ von Star!

sun = Star("Sonne")
earth = Planet("Erde")
```

#### Zeile 1: `print(sun)` ➔ Ausgabe: `S:Sonne`

- `sun` ist ein Objekt der Klasse `Star`.
- Ein normales `print()` sucht zuerst nach `__str__`.
- `Star` hat `__str__` definiert (gibt `S:Sonne` zurück).

#### Zeile 2: `print(earth)` ➔ Ausgabe: `P:Erde`

- `earth` ist ein Objekt der Klasse `Planet`.
- `print()` sucht nach `__str__`.
- `Planet` hat eine eigene `__str__`-Methode definiert (gibt `P:Erde` zurück).

#### Zeile 3: `print([earth])` ➔ Ausgabe: `[R:Erde]` (Die große PCAP-Falle!)

- **Achtung:** `earth` steckt hier in einer Liste `[]`!
- Wenn Python eine Liste druckt, ignoriert es `__str__` für die Elemente komplett. Es verlangt für jedes Element zwingend die **`__repr__`**-Methode.
- Python schaut in die Klasse `Planet`. Dort gibt es kein `__repr__`.
- Wegen der Vererbung wandert Python hoch zur Elternklasse `Star`. Dort findet es `__repr__` (gibt `R:Erde` zurück).

➔ **Die richtige Antwort im Quiz wäre also C gewesen (`S:Sonne P:Erde [R:Erde]`).**

---

### 3. Zusammenfassung für den Test

| Aufruf im Code              | Python sucht zuerst... | Fallback bei Fehlen                       |
| :-------------------------- | :--------------------- | :---------------------------------------- |
| `print(obj)`                | `__str__`              | `__repr__`                                |
| `str(obj)`                  | `__str__`              | `__repr__`                                |
| `print([obj])` _(in Liste)_ | `__repr__`             | Standard-Python-Adresse (`<__main__...>`) |
| `repr(obj)`                 | `__repr__`             | Standard-Python-Adresse (`<__main__...>`) |

⚠️ **Zwei wichtige Zusatzregeln für PCAP:**

1. Beide Methoden **müssen** immer einen **String (Text)** zurückgeben (`return`). Wenn du dort eine Zahl zurückgibst (z. B. `return 123`), wirft Python sofort einen **`TypeError`**.
2. Gibt es gar keine dieser Methoden, nutzt Python den Standard von `object` und gibt so etwas aus wie `<__main__.Planet object at 0x0042>`.

> name mangeling

```python
class Stack:
    def __init__(self):
        self.__stk = []  # Intern: _Stack__stk

    def push(self, val):
        self.__stk.append(val)

class CountingStack(Stack):
    def __init__(self):
        super().__init__()
        self.__counter = 0  # Intern: _CountingStack__counter

# Objekt erstellen und befüllen
stk = CountingStack()
stk.push("PCAP Zertifikat")

# --- DER BEWEIS ---

# 1. Normaler Zugriff (Schlägt fehl!)
try:
    print(stk.__stk)
except AttributeError:
    print("Zugriff verweigert: stk.__stk existiert nicht!")

# 2. Zugriff über Name-Mangling (Funktioniert!)
# Muster: _KlassennameInDerEsDefiniertWurde__variablenname
print("Inhalt der Liste:", stk._Stack__stk)          # Ausgabe: ['PCAP Zertifikat']
print("Wert des Zählers:", stk._CountingStack__counter) # Ausgabe: 0

"""
Möchtest du sehen, wie wir den Inhalt des Stacks von außerhalb des Codes
über einen "gemangelten" Namen ausgeben können, um das im Terminal zu beweisen?

Der Trick: Du nimmst das Objekt (stk), schreibst einen einzelnen Unterstrich,
dann den Namen der Klasse, in der die Variable geboren wurde, gefolgt von
zwei Unterstrichen und dem Variablennamen (stk._Stack__stk).
"""

"""
class CyberTech:
    version = "1.0"

    def __init__(self, identifier):
        self.id = identifier
        self.__status = "active"

device1 = CyberTech("A1")
device2 = CyberTech("B2")

CyberTech.version = "1.1"
device1.__status = "inactive"
device1.version = "2.0"
"""

"""
Zugriff verweigert: stk.__stk existiert nicht!
Inhalt der Liste: ['PCAP Zertifikat']
Wert des Zählers: 0
"""

```

> properties example class

```python
class ExampleClass:
    varia = 1
    def __init__(self, val):
        ExampleClass.varia = val

print(ExampleClass.__dict__)
example_object = ExampleClass(2)

print(ExampleClass.__dict__)
print(example_object.__dict__)

print(example_object.varia)

print("*******************************************")

class TestClass:
    x = 10
    def __init__(self, val):
        TestClass.x = val

obj1 = TestClass(20)
obj2 = TestClass(30)
obj1.x = 40

print(obj1.x, obj2.x, TestClass.x)


"""
{'__module__': '__main__', 'varia': 1, '__init__': <function ExampleClass.__init__ at 0x000002DDB0338680>, '__dict__': <attribute '__dict__' of 'ExampleClass' objects>, '__weakref__': <attribute '__weakref__' of 'ExampleClass' objects>, '__doc__': None}
{'__module__': '__main__', 'varia': 2, '__init__': <function ExampleClass.__init__ at 0x000002DDB0338680>, '__dict__': <attribute '__dict__' of 'ExampleClass' objects>, '__weakref__': <attribute '__weakref__' of 'ExampleClass' objects>, '__doc__': None}
{}
2
*******************************************
40 30 30
"""

```

> oop lifo

```python
class Stack:
    def __init__(self):
        self.stack_list = []
        self.sum = 0

    def push(self, value):
        self.stack_list.append(value)


    def pop(self):
        #value = self.stack_list.pop()
        value = self.stack_list[-1]
        del self.stack_list[-1]
        return value

    def get_stack(self):
        return self.stack_list

class AddingStack(Stack):
    def __init__(self):
        Stack.__init__(self)
        #self.sum = 0


    def push(self, value):
        self.sum += value
        Stack.push(self, value)

    def pop(self):
        value = self.stack_list[-1]
        self.sum -= value
        del self.stack_list[-1]

    def get_sum(self):
        return self.sum


lifo2 = AddingStack()
lifo2.push(123)
print(lifo2.get_stack())
print(lifo2.get_sum())
lifo2.pop()
print(lifo2.get_sum())
print(lifo2.sum)

"""
lifo1 = Stack()
lifo1.push(5)
print(lifo1.get_stack())
lifo1.pop()
print(lifo1.get_stack())

for i in range(5):
    lifo1.push(i)
print(lifo1.get_stack())
print(self.sum)


for i in range(5):
    lifo1.pop()
print(lifo1.get_stack())
"""

"""
[123]
123
0
0
"""

```
