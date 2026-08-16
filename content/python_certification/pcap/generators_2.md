+++
date = '2026-08-15T18:18:39+02:00'
draft = false
title = 'Python_generators_2'
showAuthor = false
weight =24
layout = "simple"
summary = "🚀 Python_generators_2"
+++

> code

```python
import sys

x = range(1, 11)

print(x)    # range(1, 11)

#iter()     --> returns and iterator from an object
# .__iter__()   # Dunder-Method

print(next(iter(x)))

"""
liste = [1, 2, 3]

# print(next(liste))
# Fehlermeldung! TypeError: 'list' object is not an iterator

# So funktioniert es:
listen_iterator = iter(liste)
print(next(listen_iterator))  # Ausgabe: 1
print(next(listen_iterator))  # Ausgabe: 2
"""

class Iter:
    def __init__(self, n):
        self.n = n

    def __iter__ (self):
        # Start at -1 so the first increment makes it 0
        self.current = -1
        return self

    def __next__(self):
        # Corrected from =+1 to += 1
        self.current += 1

        if self.current >= self.n:
            raise StopIteration

        return self.current

x = Iter(5)

#for i in x:
    #print(i)

itr = iter(x)
print(next(x))  # 0
print(next(x))  # 1
print(next(x))  # 2

```

> Erklärung

---

title: "Die ultimative Erklärung: Python Iteratoren Schritt für Schritt"
date: 2026-08-15
draft: false
description: "Ein anfängerfreundlicher Guide zu Iteratoren in Python, perfekt erklärt für die PCAP-Prüfung."
showToc: true
tocOpen: true

---

Hier ist der Code. Ich erkläre jetzt jede einzelne Zeile so, dass kein Platz mehr für Rätsel ist.

```python
class Iter:
```

> **Erklärung:** Hier rufen wir den Bauplan ins Leben. Wir taufen ihn `Iter`. Ab jetzt weiß der Computer: Alles, was eingerückt darunter steht, gehört zu diesem Bauplan.

```python
    def __init__(self, n):
        self.n = n
```

> **Erklärung:** Das ist der Vorbereitungs-Knopf. Sobald du später `Iter(5)` schreibst, wird dieser Knopf gedrückt.
>
> - Das Wort `self` ist wie ein Korb, der zu genau diesem einen Objekt gehört.
> - `self.n = n` bedeutet: "Lege die Zahl 5 in unseren Korb unter dem Namen `n` ab, damit wir uns später an sie erinnern."

```python
    def __iter__ (self):
        self.current = -1
        return self
```

> **Erklärung:** Das ist der Start-Knopf für die Schleife. Sobald die `for`-Schleife das Objekt `x` sieht, drückt sie einmalig diesen Knopf.
>
> - `self.current = -1`: Wir legen eine neue Variable in unseren Korb. Sie heißt `current` (aktueller Wert) und startet bei `-1`. Warum? Weil wir gleich als erstes eine 1 draufrechnen wollen, um bei 0 zu landen.
> - `return self`: Das bedeutet einfach: "Ich habe mich erfolgreich vorbereitet und bin bereit. Du kannst mich jetzt benutzen."

```python
    def __next__(self):
        self.current += 1
```

> **Erklärung:** Das ist der "Weiter"-Knopf. Die `for`-Schleife drückt diesen Knopf jetzt in Dauerschleife (Runde für Runde).
>
> - `self.current += 1`: Wir nehmen den aktuellen Wert aus dem Korb (beim ersten Mal die `-1`) und zählen 1 dazu. Aus `-1` wird 0. In der nächsten Runde wird aus 0 eine 1, dann eine 2, und so weiter.

```python
        if self.current >= self.n:
            raise StopIteration
```

> **Erklärung:** Das ist die Notbremse. Wir schauen in unseren Korb: Ist die aktuelle Zahl (`self.current`) schon so groß oder größer wie unser Limit (`self.n`, also die 5)?
>
> - Wenn `self.current` irgendwann 5 wird, ist `5 >= 5` wahr (`True`).
> - `raise StopIteration`: Das ist ein eingebauter Schock-Befehl für Python. Es wirft einen "Fehler" aus, den die `for`-Schleife aber versteht als: "HALT! Sofort aufhören!" Die Schleife bricht augenblicklich ab.

```python
        return self.current
```

> **Erklärung:** Wenn die Notbremse oben nicht gezogen wurde, werfen wir die aktuelle Zahl (z. B. 0) aus der Methode heraus. Die `for`-Schleife fängt diese Zahl auf.

---

## Der Ablauf im Finale (Das Zusammenspiel)

Jetzt setzen wir den Bauplan ein:

```python
x = Iter(5)
```

> **Was passiert?** Python baut das Objekt `x`. Es drückt `__init__`. Im Korb von `x` ist jetzt gespeichert: `n = 5`.

```python
for i in x:
    print(i)
```

### Das große Finale Schritt für Schritt:

1. Die `for`-Schleife startet. Sie drückt heimlich den Knopf `__iter__`. Im Korb wird `current = -1` erstellt.
2. **Runde 1:** Die Schleife drückt den Knopf `__next__`.
   - `current` wird von -1 auf 0 erhöht.
   - Ist `0 >= 5`? Nein.
   - Die 0 wird zurückgegeben. `i` wird zu 0. `print(0)` gibt **0** aus.
3. **Runde 2:** Die Schleife drückt wieder `__next__`.
   - `current` wird von 0 auf 1 erhöht.
   - Ist `1 >= 5`? Nein.
   - Die 1 wird zurückgegeben. `print(1)` gibt **1** aus.
4. _(Das Gleiche passiert für 2, 3 und 4...)_
5. **Die Endrunde:** Die Schleife drückt noch einmal `__next__`.
   - `current` wird von 4 auf 5 erhöht.
   - Ist `5 >= 5`? **Ja!**
   - `raise StopIteration` wird ausgelöst. Die Schleife hört sofort auf. Das Programm ist zu Ende.

> iterable vs. iterator

---

title: "PCAP-Wissen: Iterable vs. Iterator und das Geheimnis von 'return self'"
date: 2026-08-15
draft: false
description: "Der feine Unterschied zwischen Iterable und Iterator in Python sowie die Auswirkung von fehlendem 'return self'."
showToc: true
tocOpen: true

---

Das Wichtigste zuerst für die PCAP-Prüfung: Python unterscheidet streng zwischen zwei Begriffen:

- **Das Iterable:** Das Objekt, das man durchlaufen _kann_ – es besitzt die `__iter__`-Methode.
- **Der Iterator:** Das Objekt, das die eigentliche Arbeit macht und die Runden zählt – es besitzt die `__next__`-Methode.

In deinem Code ist das Objekt `x` **beides gleichzeitig**.

Wenn die `for`-Schleife startet, fragt sie `x`: _"Wer ist der Iterator, der für mich die Runden zählt?"_. Durch `return self` antwortet das Objekt: _"Ich mache das selbst! Ich habe die `__next__`-Methode an Bord, du kannst mich direkt fragen!"_

---

## Was würde passieren, wenn `return self` fehlt?

Wenn du das `return self` vergisst oder `None` zurückgibst, passiert Folgendes:

1. Die `for`-Schleife startet und ruft brav `__iter__` auf.
2. Die Schleife erwartet jetzt ein Objekt, bei dem sie als Nächstes den `__next__`-Knopf drücken kann.
3. Wenn da aber kein Objekt zurückkommt (`None`), weiß Python nicht, wo es `__next__` aufrufen soll.

Das Programm stürzt sofort mit dieser Fehlermeldung ab:

```text
TypeError: iter() returned non-iterator of type 'NoneType'
```

> Fib generator

```python
class Fib:
    def __init__(self, nn):
        print("__init__")
        self.__n = nn
        self.__i = 0
        self.__p1 = self.__p2 = 1

    def __iter__(self):
        print("__iter__")
        return self

    def __next__(self):
        print("__next__")
        self.__i += 1
        if self.__i > self.__n:
            raise StopIteration
        if self.__i in [1, 2]:
            return 1
        ret = self.__p1 + self.__p2
        self.__p1, self.__p2 = self.__p2, ret
        return ret

for i in Fib(10):
    print(i)
```

> Fib generator composition

```python
class Fib:
    def __init__(self, nn):
        self.__n = nn
        self.__i = 0
        self.__p1 = self.__p2 = 1

    def __iter__(self):
        print("Fib iter")
        return self

    def __next__(self):
        self.__i += 1
        if self.__i > self.__n:
            raise StopIteration
        if self.__i in[1,2]:
            return 1
        ret = self.__p1 + self.__p2
        self.__p1, self.__p2 = self.__p2, ret
        return ret


class Class:
    def __init__(self, n):
        self.__iter = Fib(n)

    def __iter__(self):
        print("Class iter")
        return self.__iter


object = Class(8)

for i in object:
    print(i)
```
