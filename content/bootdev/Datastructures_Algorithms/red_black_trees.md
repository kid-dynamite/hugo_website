+++
date = '2026-09-20T16:32:54+02:00'
draft = false
title = 'Red Black Trees'
showAuthor = false
weight =14
layout = "simple"
summary = "🚀 Red Black Trees"
+++

> Fix Insert Beispiel

# Visualisierung: fix_insert im Rot-Schwarz-Baum

Wir spielen das einfachste Szenario durch: Wir erzeugen einen **"Zick-Zack"-Konflikt (Right-Left)**, der sowohl das Umfärben als auch die Rotationen triggert.

## Die Ausgangslage

Wir haben bereits zwei Zahlen eingefügt: **10** und **20**. Der Baum sieht im Speicher so aus:

- **10** ist die Wurzel (`self.root`) und ist **SCHWARZ**.
- **20** hängt rechts an der 10 (`10.right`) und ist **ROT** (weil neue Knoten immer rot starten).

```text
    10 (Schwarz)
      \
       20 (Rot)
```

---

## Der Start: Wir fügen die 15 ein

Jetzt rufen wir `insert(15)` auf:

1.  **Einfügen:** Die `insert`-Methode läuft durch. Die 15 landet links unter der 20 (`20.left`).
2.  **Farbe:** Die 15 startet als **ROT**.

So sieht der Baum direkt nach dem Einfügen aus (Zick-Zack-Form):

```text
    10 (Schwarz)
      \
       20 (Rot)
      /
    15 (Rot)  <-- Neuer Knoten
```

> [!CAUTION]
> **Das Problem:** Wir haben jetzt zwei rote Knoten direkt untereinander (20 ist rot, 15 ist rot). Das ist im Rot-Schwarz-Baum strengstens verboten!

**Der Aufruf:** Am Ende von `insert` wird `self.fix_insert(new_node)` aufgerufen (wobei `new_node` die 15 ist).

---

## Jetzt geht fix_insert los (Schritt für Schritt)

### 1. Die Schleifen-Bedingung prüfen

```python
while self.root is not new_node and new_node.parent.red:
```

- Ist `new_node` (15) die Wurzel? **Nein** (Wurzel ist 10).
- Ist der Vater (20) rot? **Ja!**
- **Ergebnis:** Wir gehen in die `while`-Schleife rein.

### 2. Wer ist wer? (Die Verwandten bestimmen)

```python
parent = new_node.parent        # parent ist die 20
grandparent = parent.parent     # grandparent ist die 10
```

### 3. Die Weichenstellung (Sind wir links oder rechts?)

```python
if parent is grandparent.right:
```

- Ist `parent` (20) das rechte Kind von `grandparent` (10)? **Ja!**
- Wir gehen also in den ersten großen Hauptblock.

### 4. Wer ist der Onkel?

```python
uncle = grandparent.left
```

- Was hängt links an der 10? Da ist noch nichts, also zeigt `grandparent.left` auf `self.nil`.
- Unser `uncle` ist also `self.nil`.

### 5. Onkel-Check (Fall A oder Fall B?)

```python
if uncle.red:
```

- Ist der Onkel (`self.nil`) rot? **Nein, `self.nil` ist immer schwarz!**
- Der `if`-Block wird übersprungen. Wir landen im `else`-Block (**Fall B: Onkel ist schwarz**).

### 6. Das Zick-Zack auflösen (Die erste Rotation)

Wir sind jetzt hier im Code:

```python
if new_node is parent.left:
    new_node = parent
    self.rotate_right(new_node)
    parent = new_node.parent
    grandparent = parent.parent
```

- Ist `new_node` (15) das left Kind von `parent` (20)? **Ja, es ist ein Zick-Zack!**
- `new_node = parent`: Unsere Variable `new_node` springt hoch und zeigt jetzt auf die 20.
- `self.rotate_right(20)`: Wir rotieren die 20 nach rechts unten.
  - _Was passiert im Baum?_ Die 15 rutscht hoch und wird der neue Vater der 20.
  - Die 10 zeigt jetzt rechts auf die 15, und unter der 15 hängt rechts die 20.
- **Wichtig für den Fix:** Wir aktualisieren die Verwandten. `parent` ist jetzt die 15, `grandparent` ist immer noch die 10.

Aus dem Zick-Zack (`10 -> 20 -> 15`) ist jetzt eine **gerade Linie nach rechts** geworden:

```text
    10 (Schwarz)
      \
       15 (Rot)
         \
          20 (Rot)
```

### 7. Das Finale: Den Baum gerade biegen

Jetzt führt der Code den Rest des `else`-Blocks aus:

```python
parent.red = False           # Die 15 (parent) wird SCHWARZ
grandparent.red = True       # Die 10 (grandparent) wird ROT
self.rotate_left(grandparent) # Wir rotieren um die 10 nach links!
```

- _Was passiert bei `rotate_left(10)`?_ Die 15 (die schwarz ist) rutscht ganz nach oben und wird die neue **Wurzel** des Baums.
- Links unter der 15 hängt jetzt die 10 (die rot ist).
- Rechts unter der 15 hängt die 20 (die rot ist).

### 8. Die Schleife endet

Im nächsten Durchlauf prüft die `while`-Schleife:

- Ist `new_node.parent.red` noch wahr?
- `new_node` ist immer noch das Objekt 20. Dessen Vater ist jetzt die 15. Die 15 ist aber schwarz!
- Die Bedingung ist **falsch**. Die Schleife bricht ab.

Ganz am Ende der Methode sorgt `self.root.red = False` dafür, dass die neue Wurzel (15) garantiert schwarz bleibt.

---

## Das Ergebnis im Kopf

Wir haben mit einer schiefen Kette angefangen und enden mit einem perfekt balancierten Dreieck:

```text
      15 (Schwarz)
     /  \
    10    20   (Beide Rot)
```

Mit diesem kleinen 3-Schritte-Szenario hast du im Kopf exakt die komplizierteste Route genommen, die der Code anbietet (Zick-Zack auflösen, Umfärben, finale Rotation).

> Fix Insert

```python
from typing import Any


class RBNode:
    def __init__(self, val: Any) -> None:
        self.red = False
        self.parent: RBNode | None = None
        self.val = val
        self.left: RBNode = self
        self.right: RBNode = self


class RBTree:
    def __init__(self) -> None:
        self.nil = RBNode(None)
        self.nil.red = False
        self.nil.left = self.nil
        self.nil.right = self.nil
        self.root = self.nil

    def insert(self, val: Any) -> None:
        new_node = RBNode(val)
        new_node.parent = None
        new_node.left = self.nil
        new_node.right = self.nil
        new_node.red = True

        parent: RBNode | None = None
        current = self.root
        while current != self.nil:
            parent = current
            if new_node.val < current.val:
                current = current.left
            elif new_node.val > current.val:
                current = current.right
            else:
                # duplicate, just ignore
                return

        new_node.parent = parent
        if parent is None:
            self.root = new_node
        elif new_node.val < parent.val:
            parent.left = new_node
        else:
            parent.right = new_node

        self.fix_insert(new_node)




    def fix_insert(self, new_node: RBNode) -> None:
        while self.root  is not new_node and new_node.parent.red:
            parent = new_node.parent
            grandparent = new_node.parent.parent
            if parent is grandparent.right:
                uncle = grandparent.left
                if uncle.red:
                    uncle.red = False
                    parent.red = False
                    grandparent.red = True
                    new_node = grandparent
                else:
                    if new_node is parent.left:
                        new_node = parent
                        self.rotate_right(new_node)
                        parent = new_node.parent
                    parent.red = False
                    grandparent.red = True
                    self.rotate_left(grandparent)
            elif parent is grandparent.left:
                uncle = grandparent.right
                if uncle.red:
                    uncle.red = False
                    parent.red = False
                    grandparent.red = True
                    new_node = grandparent
                else:

                    if new_node is parent.right:
                        new_node = parent
                        self.rotate_left(new_node)
                        parent = new_node.parent
                    parent.red = False
                    grandparent.red = True
                    self.rotate_right(grandparent)
        self.root.red = False











    def exists(self, val: Any) -> RBNode:
        curr = self.root
        while curr != self.nil and val != curr.val:
            if val < curr.val:
                curr = curr.left
            else:
                curr = curr.right
        return curr

    def rotate_left(self, pivot_parent: RBNode) -> None:
        if pivot_parent == self.nil or pivot_parent.right == self.nil:
            return
        pivot = pivot_parent.right
        pivot_parent.right = pivot.left
        if pivot.left != self.nil:
            pivot.left.parent = pivot_parent

        pivot.parent = pivot_parent.parent
        if pivot_parent.parent is None:
            self.root = pivot
        elif pivot_parent == pivot_parent.parent.left:
            pivot_parent.parent.left = pivot
        else:
            pivot_parent.parent.right = pivot
        pivot.left = pivot_parent
        pivot_parent.parent = pivot

    def rotate_right(self, pivot_parent: RBNode) -> None:
        if pivot_parent == self.nil or pivot_parent.left == self.nil:
            return
        pivot = pivot_parent.left
        pivot_parent.left = pivot.right
        if pivot.right != self.nil:
            pivot.right.parent = pivot_parent

        pivot.parent = pivot_parent.parent
        if pivot_parent.parent is None:
            self.root = pivot
        elif pivot_parent == pivot_parent.parent.right:
            pivot_parent.parent.right = pivot
        else:
            pivot_parent.parent.left = pivot
        pivot.right = pivot_parent
        pivot_parent.parent = pivot


# 1. Erstelle eine Instanz deines Baums
mein_baum = RBTree()

# 2. Befülle den Baum mit Werten über die insert-Methode
werte = [15, 10, 20, 5, 12, 18, 25]

for wert in werte:
    mein_baum.insert(wert)

# 3. Überprüfen, ob ein Wert im Baum existiert
gesuchter_wert = 12
ergebnis_knoten = mein_baum.exists(gesuchter_wert)

if ergebnis_knoten != mein_baum.nil:
    print(f"Wert {gesuchter_wert} wurde im Baum gefunden!")
else:
    print(f"Wert {gesuchter_wert} existiert nicht im Baum.")
```

> Rotation

```python
from typing import Any


class RBNode:
    def __init__(self, val: Any) -> None:
        self.red = False
        self.parent: RBNode | None = None
        self.val = val
        self.left: RBNode = self
        self.right: RBNode = self


class RBTree:
    def __init__(self) -> None:
        self.nil = RBNode(None)
        self.nil.red = False
        self.nil.left = self.nil
        self.nil.right = self.nil
        self.root = self.nil

    def rotate_left(self, pivot_parent: RBNode) -> None:
        # 1. SICHERHEITS-CHECK (Guard Clause)
        # Wenn der aktuelle Knoten oder sein rechtes Kind ein NIL-Knoten ist,
        # können wir nicht nach links rotieren. Wir brechen sofort ab.
        if pivot_parent is self.nil or pivot_parent.right is self.nil:
            return

        # 2. PIVOT SICHERN
        # Wir merken uns das rechte Kind als den Dreh- und Angelpunkt ("pivot").
        pivot = pivot_parent.right

        # 3. LINKEN TEILBAUM VOM PIVOT UMSETZEN
        # Das linke Kind des Pivots wird zum neuen rechten Kind von pivot_parent.
        pivot_parent.right = pivot.left

        # 4. ELTERN-ZEIGER DES UMGEHÄNGTEN TEILBAUMS UPDATEN
        # Falls das linke Kind von pivot kein NIL-Blatt ist, muss dessen
        # Vater-Zeiger jetzt auf pivot_parent zeigen.
        if pivot.left is not self.nil:
            pivot.left.parent = pivot_parent

        # 5. PIVOT EINE EBENE HÖHER BRINGEN
        # Der pivot übernimmt den alten Vater von pivot_parent.
        pivot.parent = pivot_parent.parent

        # 6. DEN GROSSVATER ANPASSEN (Wer zeigt jetzt von oben auf den Pivot?)
        if pivot_parent.parent is None:
            # Wenn pivot_parent keinen Vater hatte, war er die Wurzel.
            # Der pivot ist nun die neue Wurzel des gesamten Baumes.
            self.root = pivot
        elif pivot_parent is pivot_parent.parent.left:
            # War pivot_parent das LINKE Kind seines Vaters,
            # zeigt der Vater jetzt auf pivot als neues LINKES Kind.
            pivot_parent.parent.left = pivot
        elif pivot_parent is pivot_parent.parent.right:
            # War pivot_parent das RECHTE Kind seines Vaters,
            # zeigt der Vater jetzt auf pivot als neues RECHTES Kind.
            pivot_parent.parent.right = pivot

        # 7. DIE NEUE HIERARCHIE ZWISCHEN PIVOT UND PIVOT_PARENT
        # pivot_parent rutscht nach unten und wird das LINKE Kind von pivot.
        pivot.left = pivot_parent

        # 8. ELTERN-ZEIGER SCHLIESSEN
        # Der neue Vater von pivot_parent ist jetzt der pivot.
        pivot_parent.parent = pivot


    def rotate_right(self, pivot_parent: RBNode) -> None:
        # Hier kommt die gespiegelte Logik rein!

```

> Erklärungen

```python
# 1. Erstelle EINE Instanz des gesamten Baumes
mein_baum = RBTree()

# 2. Füge Zahlen hinzu. Der Baum baut die RBNode-Instanzen selbst!
mein_baum.insert(10)  # Das wird automatisch die Root-Instanz
mein_baum.insert(5)   # Das wird automatisch das linke Kind
mein_baum.insert(15)  # Das wird automatisch das rechte Kind


inst1 = RBNode(1)
inst1 = RBNode(2)  # Überschreibt inst1!




# 1. Baum erstellen und Werte einfügen
mein_baum = RBTree()
mein_baum.insert(10)
mein_baum.insert(5)
mein_baum.insert(20)

# 2. Die Wurzel (Root) abfragen
print(mein_baum.root.val)        # Gibt 10 aus

# 3. Die Kinder der Wurzel abfragen
print(mein_baum.root.left.val)   # Gibt 5 aus (Instanz B)
print(mein_baum.root.right.val)  # Gibt 20 aus (Instanz C)

# 4. Den Spieß umdrehen: Wer ist das Elternteil der 5?
print(mein_baum.root.left.parent.val) # Gibt wieder 10 aus!

```
