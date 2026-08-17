+++
date = '2026-08-16T23:11:35+02:00'
draft = false
title = 'lambda_map_filter_closures'
showAuthor = false
weight =26
layout = "simple"
summary = "🚀 lambda_map_filter_closures"
+++

> lambda

### 1. Was ist eine Lambda-Funktion?

In Python ist eine **Lambda-Funktion** eine kleine, anonyme Funktion (eine Funktion ohne Namen). Sie wird meistens für kurze, einmalige Operationen verwendet, bei denen sich die Definition einer regulären Funktion mit `def` nicht lohnt.

Die grundlegende Syntax lautet:

```python
lambda parameter1, parameter2, ... : ausdruck
```

---

### 2. Die goldenen PCAP-Regeln für Lambdas

Im PCAP-Test werden oft syntaktische Fallen gestellt. Merke dir diese vier Regeln:

1. **Kein `return`-Statement:** Ein Lambda gibt den Wert des Ausdrucks _automatisch_ zurück. Das Wort `return` im Lambda führt zu einem `SyntaxError`.
2. **Nur EIN Ausdruck:** Nach dem Doppelpunkt darf nur ein einziger Ausdruck stehen. Du kannst dort keine Schleifen (`for`/`while`) oder Zuweisungen (`x = 5`) platzieren.
3. **Beliebige Anzahl an Argumenten:** Ein Lambda kann keine, eine oder beliebig viele Variablen entgegennehmen (sogar `*args` und `**kwargs` sind erlaubt).
4. **Sofortige Ausführung (IIFE):** Ein Lambda kann direkt bei seiner Definition aufgerufen werden, indem man es in Klammern setzt.

---

### 3. Typische PCAP-Prüfungsszenarien

#### Szenario A: Die syntaktische Falle (Falsch vs. Richtig)

In der Prüfung zeigen sie dir oft Code wie diesen und fragen, was passiert:

```python
# ❌ SYNTAX-ERROR in der Prüfung!
mein_fehler = lambda x: return x * 2

#  KORREKT
verdoppeln = lambda x: x * 2
print(verdoppeln(5))  # Ausgabe: 10
```

#### Szenario B: Beliebige Anzahl an Argumenten

Ein Lambda kann auch komplett ohne Argumente existieren oder mehrere Variablen verarbeiten:

```python
# Ohne Argumente
immer_wahr = lambda: True
print(immer_wahr())  # Ausgabe: True

# Mit mehreren Argumenten
potenz = lambda basis, exponent: basis ** exponent
print(potenz(2, 3))  # Ausgabe: 8
```

#### Szenario C: Sofortiger Aufruf (In-place Execution)

Manchmal wird ein Lambda in der Prüfung direkt mit Parametern gefüttert, ohne es vorher einer Variablen zuzuweisen:

```python
# Das Lambda wird definiert und sofort mit der (5, 3) aufgerufen
ergebnis = (lambda x, y: x - y)(5, 3)

print(ergebnis)  # Ausgabe: 2
```

{{< alert "important" >}}
**Kombination mit `map()` und `filter()`:**
In fast 90 % der Prüfungsfragen tauchen Lambdas als das erste Argument in `map(lambda x: ..., liste)` oder `filter(lambda x: ..., liste)` auf. Wenn du verstehst, wie das Lambda ein einzelnes Element transformiert (oder filtert), hast du die Punkte sicher.
{{< /alert >}}

---

### Zusammenfassung für die Prüfung (Spickzettel)

- **`lambda` statt `def`:** Erstellt eine anonyme Funktion im Einzeiler-Format.
- **Syntax-Check:** Achte in der Prüfung darauf, dass kein `return` im Lambda steht und der Doppelpunkt `:` richtig gesetzt ist.
- **Rückgabetyp:** Ein Lambda-Ausdruck selbst gibt ein Funktionsobjekt (`<function <lambda> at ...>`) zurück, erst der Aufruf liefert das Ergebnis.
- **Gültigkeit:** Lambdas können auf Variablen außerhalb ihres eigenen Scopes zugreifen (Closures), was ebenfalls gerne in fortgeschrittenen PCAP-Fragen getestet wird.

> map()

### 1. Die grundlegende Syntax

Die `map()`-Funktion wendet eine vorgegebene Funktion auf jedes Element eines iterierbaren Objekts (z. B. Liste, Tupel, String) an.

```python
map(funktion, iterierbares_objekt)
```

---

### 2. Das wichtigste Prüfungsdetail: Was gibt `map()` zurück?

Ein extrem häufiger Stolperstein im PCAP-Test: **`map()` gibt keine Liste zurück**, sondern ein **Map-Objekt** (einen Generator/Iterator). Die Berechnung erfolgt "lazy" (verzögert), also erst, wenn die Werte abgerufen werden.

```python
# Direktes Drucken zeigt nur das Objekt, nicht die Werte
print(map(str, [1, 2]))
# Ausgabe ähnlich wie: <map object at 0x7f81a2b3c4d0>
```

{{< alert "warning" >}}
**Wichtig für die Website-Ansicht:** Um die echten Werte zu sehen oder zu nutzen, musst du das Objekt konvertieren (z. B. mit `list()`) oder per Schleife durchlaufen.
{{< /alert >}}

---

### 3. Die 3 typischen PCAP-Szenarien

#### Szenario A: Kombination mit `lambda` (Sehr häufig!)

In der Prüfung wird `map()` meistens mit einer anonymen Lambda-Funktion kombiniert.

```python
zahlen = [1, 2, 3, 4]

# Jede Zahl soll verdoppelt werden
ergebnis = map(lambda x: x * 2, zahlen)

print(list(ergebnis))
# Ausgabe: [2, 4, 6, 8]
```

#### Szenario B: Verwendung von Built-in-Funktionen

Du kannst auch bestehende Python-Funktionen übergeben. Achte darauf, die Funktion **nur beim Namen zu nennen** (ohne Klammern `()`).

```python
strings = ["10", "20", "30"]

# Konvertiert jeden String in ein Integer
zahlen = list(map(int, strings))

print(zahlen)
# Ausgabe: [10, 20, 30]
```

#### Szenario C: Mehrere Listen gleichzeitig verarbeiten

Wenn die übergebene Funktion \(N\) Argumente erwartet, kannst du `map()` auch \(N\) Listen übergeben. Die Elemente werden dann parallel verarbeitet.

{{< alert "important" >}}
**Achtung bei ungleichen Längen:** `map()` stoppt automatisch, sobald die **kürzeste** Liste abgearbeitet ist.
{{< /alert >}}

```python
liste1 = [1, 2, 3, 4]
liste2 = [10, 20, 30]  # Kürzere Liste!

# Die Lambda-Funktion nimmt zwei Argumente (x von liste1, y von liste2)
ergebnis = list(map(lambda x, y: x + y, liste1, liste2))

print(ergebnis)
# Ausgabe: [11, 22, 33] (Die 4 wird ignoriert!)
```

---

### Zusammenfassung für die Prüfung (Spickzettel)

- **Keine direkte Liste:** `map()` erzeugt einen Iterator. Direktes Drucken zeigt nur die Speicheradresse.
- **Keine Klammern bei der Funktionsübergabe:** `map(int, ...)` ist richtig, `map(int(), ...)` ist falsch.
- **Kürzeste Liste gewinnt:** Bei mehreren Iterables bricht `map()` beim kürzesten Objekt ab.
- **Zustand nach Benutzung:** Da es ein Iterator ist, ist das `map`-Objekt nach einmaligem Durchlaufen (z. B. nach `list(ergebnis)`) "leer" konsumiert.

> filter()

### 1. Die grundlegende Syntax

Die `filter()`-Funktion filtert Elemente aus einem iterierbaren Objekt (z. B. Liste, Tupel) basierend auf einer Bedingung. Sie nimmt eine Funktion, die `True` oder `False` zurückgibt, und wendet sie auf jedes Element an.

```python
filter(funktion, iterierbares_objekt)
```

---

### 2. Das wichtigste Prüfungsdetail: Was gibt `filter()` zurück?

Genau wie `map()` arbeitet auch `filter()` mit **Lazy Evaluation** (verzögerter Berechnung).

- **Keine direkte Liste:** `filter()` gibt ein **Filter-Objekt** (einen Iterator) zurück.
- `print(filter(lambda x: x > 0, [1, -2]))` gibt nur `<filter object at 0x...>` aus.
- Du musst das Ergebnis erst in eine `list()`, ein `tuple()` konvertieren oder per Schleife auslesen.

---

### 3. Die 3 typischen PCAP-Szenarien

#### Szenario A: Kombination mit `lambda` (Der Standard)

In der Prüfung filterst du meistens Zahlen oder Strings mit einer Lambda-Bedingung.

```python
zahlen = [1, 2, 3, 4, 5, 6]

# Nur gerade Zahlen durchlassen
gerade_zahlen = filter(lambda x: x % 2 == 0, zahlen)

print(list(gerade_zahlen))
# Ausgabe: [2, 4, 6]
```

#### Szenario B: Der `None`-Spezialfall (Sehr beliebte Prüfungsfrage!)

Wenn du statt einer Funktion das Keyword `None` übergibst, filtert Python alle **Falsy-Werte** (wie `0`, `False`, leere Strings `""` oder `None`) automatisch heraus.

```python
daten = [0, 1, False, "Python", "", True]

# Filtert alle "leeren" oder falschen Werte heraus
ergebnis = list(filter(None, daten))

print(ergebnis)
# Ausgabe: [1, 'Python', True]
```

{{< alert "warning" >}}
**Achtung bei der `0`:** Die Zahl `0` gilt in Python als _Falsy_ und wird bei `filter(None, ...)` gnadenlos gelöscht!
{{< /alert >}}

#### Szenario C: Filtern von Strings und Längen

Manchmal kombiniert der PCAP-Test `filter()` mit eingebauten String-Methoden oder der `len()`-Funktion.

```python
worte = ["PCAP", "Python", "AI", "Code"]

# Nur Worte durchlassen, die länger als 3 Zeichen sind
lange_worte = list(filter(lambda w: len(w) > 3, worte))

print(lange_worte)
# Ausgabe: ['PCAP', 'Python', 'Code']
```

---

### Der direkte Vergleich für die Prüfung: `map()` vs. `filter()`

Um in der Prüfung nicht durcheinanderzukommen, merke dir diesen Unterschied bei der Anwendung einer Lambda-Funktion wie `lambda x: x > 2` auf die Liste `[1, 2, 3, 4]`:

- **`map()` transformiert:** Es schaut, was die Bedingung für jedes Element ergibt.
  `list(map(lambda x: x > 2, [1, 2, 3, 4]))` $\rightarrow$ `[False, False, True, True]`
- **`filter()` siebt aus:** Es lässt nur die Elemente durch, die `True` erfüllen.
  `list(filter(lambda x: x > 2, [1, 2, 3, 4]))` $\rightarrow$ `[3, 4]`

---

### Zusammenfassung für die Prüfung (Spickzettel)

- **Wahrheitswert-Funktion:** Die übergebene Funktion muss ein Prädikat sein (gibt `True`/`False` zurück).
- **Iterator-Verhalten:** Das Filter-Objekt ist nach einmaligem Konvertieren (z. B. in eine Liste) konsumiert und "leer".
- **`None` filtert Falsy:** `filter(None, iterable)` wirft alle Nullen, leeren Strings und Booleans mit Wert `False` raus.

> closures

### 1. Was ist ein Closure?

Ein **Closure** (deutsch: Funktionsabschluss) ist eine innere Funktion, die sich an die Variablen aus ihrem umschließenden Gültigkeitsbereich (Scope) erinnert, selbst nachdem die äußere Funktion bereits vollständig ausgeführt und beendet wurde.

Damit ein Closure entsteht, müssen drei Bedingungen erfüllt sein:

1. Eine Funktion wird **innerhalb** einer anderen Funktion definiert (Verschachtelung).
2. Die innere Funktion greift auf eine Variable der äußeren Funktion zu.
3. Die äußere Funktion gibt die innere Funktion **als Objekt** zurück.

---

### 2. Die grundlegende Syntax

Hier ist das klassische Muster, wie ein Closure aufgebaut ist:

```python
def auessere_funktion(nachricht):
    # 'nachricht' ist eine lokale Variable der aeusseren Funktion
    def innere_funktion():
        # Die innere Funktion greift auf 'nachricht' zu
        print(nachricht)

    # WICHTIG: Die Funktion wird ALS OBJEKT (ohne Klammern) zurueckgegeben
    return innere_funktion

# 'mein_closure' ist jetzt die 'innere_funktion' mit einem "Gedaechtnis"
mein_closure = auessere_funktion("Hallo PCAP!")

# Die auessere_funktion ist hier laengst beendet, aber:
mein_closure()  # Ausgabe: Hallo PCAP!
```

---

### 3. Typische PCAP-Prüfungsszenarien

#### Szenario A: Der "Multiplier" (Zustände einfrieren)

In der Prüfung wird oft gezeigt, wie ein Closure verwendet wird, um maßgeschneiderte Funktionen zu generieren. Du musst berechnen können, was am Ende herauskommt.

```python
def mache_multiplikator(n):
    def multipliziere(x):
        return x * n
    return multipliziere

# n wird auf 2 bzw. 3 eingefroren
verdoppler = mache_multiplikator(2)
verdreifacher = mache_multiplikator(3)

print(verdoppler(5))     # Ausgabe: 10
print(verdreifacher(5))  # Ausgabe: 15
```

#### Szenario B: Die Falle mit dem `nonlocal`-Keyword

Wenn eine innere Funktion versucht, eine Variable der äußeren Funktion zu **verändern**, brauchst du das Keyword `nonlocal`. In der Prüfung zeigen sie dir oft Code, bei dem dieses Keyword fehlt, was zu einem Fehler führt.

```python
def zaehler_erstellen():
    zaehler = 0
    def innere():
        nonlocal zaehler  # Ohne das gibt es einen UnboundLocalError!
        zaehler += 1
        return zaehler
    return innere

mein_zaehler = zaehler_erstellen()
print(mein_zaehler())  # Ausgabe: 1
print(mein_zaehler())  # Ausgabe: 2
```

{{< alert "warning" >}}
**Unterschied zu `global`:** Das Keyword `global` verweist auf die oberste Ebene des Skripts. `nonlocal` verweist _nur_ auf den nächsthöheren, umschließenden Funktions-Scope. Für Closures ist fast immer `nonlocal` die richtige Antwort!
{{< /alert >}}

#### Szenario C: Klammern-Fehler beim Return

Achte im PCAP-Test penibel darauf, wie die äußere Funktion die innere Funktion zurückgibt.

```python
# ❌ KEIN CLOSURE (Innere Funktion wird sofort ausgeführt)
def test1():
    def innere(): return "Hi"
    return innere()  # Gibt den String "Hi" zurück!

#  ECHTES CLOSURE (Funktionsobjekt wird übergeben)
def test2():
    def innere(): return "Hi"
    return innere    # Gibt die Funktion selbst zurück!
```

---

### Zusammenfassung für die Prüfung (Spickzettel)

- **Gedächtnis-Effekt:** Ein Closure behält Zugriff auf den umschließenden Scope, auch wenn die äußere Funktion nicht mehr aktiv ist.
- **Return ohne Klammern:** Die äußere Funktion muss `return innere` schreiben, nicht `return innere()`.
- **Modifikation erfordert `nonlocal`:** Willst du die umschließende Variable in der inneren Funktion ändern, ist `nonlocal` Pflicht.
- **`__closure__`-Attribut:** Jedes Closure-Objekt besitzt in Python ein verstecktes Attribut `__closure__`, in dem die eingefrorenen Werte (in sogenannten "Cells") gespeichert sind.

> list comprehension & generator comprehension

### 1. Die grundlegende Syntax

Comprehensions bieten eine elegante, prägnante Möglichkeit, neue Iterables aus bestehenden Iterables zu erstellen. Im PCAP-Test musst du den Unterschied zwischen der Listen- und der Generator-Variante blind beherrschen.

```python
# List Comprehension (Erzeugt sofort eine Liste im Speicher)
liste = [ausdruck for element in iterable]

# Generator Comprehension / Expression (Erzeugt einen Lazy Generator)
generator = (ausdruck for element in iterable)
```

---

### 2. Das wichtigste Prüfungsdetail: Liste vs. Generator

Der größte Unterschied liegt im Speicher- und Berechnungsverhalten. In der Prüfung wird oft gefragt, was `print()` für das jeweilige Objekt ausgibt:

```python
# 1. List Comprehension
quadrate_liste = [x**2 for x in range(3)]
print(quadrate_liste)
# Ausgabe direkt: [0, 1, 4]

# 2. Generator Comprehension
quadrate_gen = (x**2 for x in range(3))
print(quadrate_gen)
# Ausgabe: <generator object <genexpr> at 0x...>
```

{{< alert "important" >}}
**Wichtig für den Test:** Ein Generator berechnet die Werte erst, wenn du ihn z. B. mit `next()`, einer `for`-Schleife oder `list()` dazu zwingst. Zudem ist ein Generator nach einmaligem Durchlaufen "leer" (konsumiert).
{{< /alert >}}

---

### 3. Typische PCAP-Prüfungsszenarien

#### Szenario A: Die Platzierung von `if` vs. `if-else` (Die größte Falle!)

Achte im Test penibel darauf, wo die Bedingungen stehen. Die Syntax ändert sich, je nachdem ob es ein reiner Filter oder eine Entweder-Oder-Logik ist.

```python
# Fall 1: REINER FILTER (if steht AM ENDE)
# Nur gerade Zahlen behalten
gerade = [x for x in range(5) if x % 2 == 0]
print(gerade)  # Ausgabe: [0, 2, 4]

# Fall 2: TRANSFORMATION (if-else steht AM ANFANG)
# Wenn gerade -> x, sonst -> "ungerade"
anzahl = [x if x % 2 == 0 else "ungerade" for x in range(3)]
print(anzahl)  # Ausgabe: [0, 'ungerade', 2]
```

{{< alert "warning" >}}
**SyntaxError im Test:** Code wie `[x for x in range(5) if x % 2 == 0 else 0]` (also ein `else` am Ende) führt sofort zu einem `SyntaxError`.
{{< /alert >}}

#### Szenario B: Verschachtelte Schleifen (Nested Loops)

In der Prüfung wird dir oft eine zweidimensionale Matrix oder eine verschachtelte Schleife in einer Zeile präsentiert. Lies sie immer stur von links nach rechts!

```python
# Eine List Comprehension mit zwei for-Schleifen:
kombination = [x + y for x in [1, 2] for y in]
print(kombination)  # Ausgabe: [11, 21, 12, 22]

# Entspricht exakt diesem klassischen Code:
# ergebnis = []
# for x in:
#     for y in:
#         ergebnis.append(x + y)
```

#### Szenario C: Der manuelle Abruf mit `next()` beim Generator

Bei Generator Comprehensions kombiniert die Prüfung den Code extrem gerne mit der eingebauten Funktion `next()`.

```python
gen = (x * 10 for x in range(5) if x > 1)

print(next(gen))  # Ausgabe: 20  (da x=2 die erste Zahl > 1 ist)
print(next(gen))  # Ausgabe: 30  (nächstes Element, x=3)
```

---

### Zusammenfassung für die Prüfung (Spickzettel)

- **Klammern bestimmen den Typ:** `[...]` macht eine Liste, `(...)` macht einen Generator.
- **Lazy Evaluation:** Generatoren sparen Arbeitsspeicher, weil sie Elemente erst bei Bedarf "on-the-fly" berechnen.
- **Filter (`if`)** steht immer ganz hinten.
- **Bedingung (`if-else`)** steht immer ganz vorne (vor dem ersten `for`).
- **Nested Loops:** Die äußere Schleife steht links, die innere Schleife steht rechts.
