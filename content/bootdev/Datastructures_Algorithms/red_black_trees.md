+++
date = '2026-09-20T16:32:54+02:00'
draft = false
title = 'Red Black Trees'
showAuthor = false
weight =14
layout = "simple"
summary = "🚀 Red Black Trees"
+++

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
