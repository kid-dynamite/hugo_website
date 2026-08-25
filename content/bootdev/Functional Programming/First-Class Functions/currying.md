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
