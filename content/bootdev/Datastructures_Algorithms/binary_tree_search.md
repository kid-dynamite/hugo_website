+++
date = '2026-09-16T22:53:03+02:00'
draft = false
title = 'Binary Tree Search'
showAuthor = false
weight =13
layout = "simple"
summary = "🚀 Binary Tree Search"
+++

> calculate hight

Unser neuer Baum sieht so aus: Die Wurzel ist **5**. Links darunter ist die **3**. Und unter der **3** hängt ganz unten links noch die **1**. Die rechte Seite lassen wir der Einfachheit halber leer (`None`), damit es übersichtlich bleibt.

```text
         5 (Wurzel – Ebene 1)
        /
       3   (Kind – Ebene 2)
      /
     1     (Blatt – Ebene 3)
```

Wir rufen jetzt `height()` ganz oben auf **Knoten 5** auf. Los geht die Reise durch den Code!

### 🟢 SCHRITT 1: Start ganz oben bei Knoten 5

Python betritt die Methode für die Wurzel (**5**):

- `if self.val is None:` ➔ Trifft nicht zu.
- `left_height = 0`, `right_height = 0`
- `if self.left is not None:` ➔ **Trifft zu!** (Da sitzt die 3).
- Python stößt auf: `left_height = self.left.height()`

> **🛑 STOPP 1:** Knoten 5 friert ein. Er weiß nicht, was rechts vom `=` rauskommt. Er geht in den Wartezustand und springt eine Ebene tiefer zu **Knoten 3**.

---

### 🟡 SCHRITT 2: Eine Ebene tiefer bei Knoten 3

Python betritt die Methode für **Knoten 3**:

- `if self.val is None:` ➔ Trifft nicht zu.
- Seine eigenen Variablen werden erstellt: `left_height = 0`, `right_height = 0`
- `if self.left is not None:` ➔ **Trifft zu!** (Da sitzt die 1).
- Python stößt wieder auf: `left_height = self.left.height()`

> **🛑 STOPP 2:** Jetzt muss auch Knoten 3 einfrieren! Er wartet ebenfalls auf die Antwort von der rechten Seite seines eigenen `=`. Er wartet auf **Knoten 1**.

---

### 🔴 SCHRITT 3: Ganz unten bei Knoten 1 (Die Sackgasse)

Python betritt die Methode für das Blatt (**1**):

- `if self.val is None:` ➔ Trifft nicht zu.
- Eigene Variablen: `left_height = 0`, `right_height = 0`
- `if self.left is not None:` ➔ **Trifft NICHT zu!** (Darunter ist nichts mehr).
- `if self.right is not None:` ➔ **Trifft NICHT zu!**

Jetzt kommt das Finale für Knoten 1:
`return max(0, 0) + 1` ➔ Das ergibt **1**.

**🎉 REISE UMKEHREN:** Knoten 1 ist fertig. Er verschwindet und wirft die **1** per `return` hoch zu seinem Chef (Knoten 3).

---

### 🟡 SCHRITT 4: Knoten 3 wacht auf

Knoten 3 taut aus dem _Stopp 2_ auf. Er fängt die **1** im Speicher auf:

- `left_height = 1` _(Das `=` hat den Wert gerade eingetragen!)_

Jetzt geht es im Code von Knoten 3 weiter:

- `if self.right is not None:` ➔ **Trifft NICHT zu!** (Rechts unter der 3 ist nichts). `right_height` bleibt also `0`.

Knoten 3 berechnet sein Ende:
`return max(left_height, right_height) + 1` ➔ `max(1, 0) + 1` ➔ `1 + 1` = **2**.

Knoten 3 schickt die **2** per `return` hoch zu seinem Chef (Knoten 5).

---

### 🟢 SCHRITT 5: Das große Finale bei der Wurzel (Knoten 5)

Knoten 5 taut aus dem _Stopp 1_ auf. Die **2** von Knoten 3 schlägt im allerersten `=` ein:

- `left_height = 2`

Jetzt läuft der Code für Knoten 5 zu Ende:

- `if self.right is not None:` ➔ **Trifft NICHT zu!** (Rechte Seite ist leer). `right_height` bleibt `0`.

Knoten 5 berechnet die finale Gesamthöhe:
`return max(left_height, right_height) + 1` ➔ `max(2, 0) + 1` ➔ `2 + 1` = **3**.

> delete node

## Das Löschen eines Knotens mit einem Kind (Die Umleitung)

Wir nehmen wieder genau denselben Baum im Speicher:

- **Knoten 10** (Wurzel) → `left` zeigt auf Knoten 5.
- **Knoten 5** (Mitte) → `left` zeigt auf Knoten 2.
- **Knoten 2** (unten) → `left` und `right` sind `None`.

Dieses Mal wollen wir die **5** löschen. Wir rufen ganz oben auf: `Knoten_10.delete(5)`

---

### Schritt 1: Fenster 1 öffnet sich (Bei Knoten 10)

Hier ist `self` der Knoten 10.

- `if val < self.val:` → Ist 5 < 10? Ja!
- `if self.left:` → Haben wir ein linkes Kind? Ja (Knoten 5).

Knoten 10 pausiert in der bekannten Zeile:

```python
self.left = self.left.delete(5)
```

**Was passiert?** Er öffnet ein zweites Fenster für Knoten 5 und wartet auf die Antwort.

---

### Schritt 2: Fenster 2 öffnet sich (Bei Knoten 5)

In diesem Fenster ist `self` nun der Knoten 5.

- `if val < self.val:` → Ist 5 < 5? Nein.
- `if val > self.val:` → Ist 5 > 5? Nein.

Der Code rutscht durch in den `else:`-Block (Wert gefunden!). Knoten 5 weiß: _„Ich muss gelöscht werden!“_ Jetzt prüft Knoten 5 seine Kinder:

```python
if self.right is None:
    return self.left
```

Da Knoten 5 kein rechtes Kind hat, ist `self.right` gleich `None`. Die Bedingung stimmt also sofort! Er gibt sein linkes Kind (`self.left`) nach oben zurück. Wer ist sein linkes Kind? Das ist **Knoten 2**.

Das zweite Fenster schließt sich und schickt das Objekt **Knoten 2** als Antwort nach oben!

---

### Der Rückweg: Die Umleitung wird gebaut

#### Zurück in Fenster 1 (Knoten 10)

Knoten 10 hat in dieser Zeile geduldig gewartet: `self.left = self.left.delete(5)`. Jetzt kommt die Antwort aus dem geschlossenen Fenster 2 an. Die Antwort lautet: **Knoten 2**.

Die Zeile wird im Speicher von Knoten 10 also exakt zu:

```python
self.left = Knoten_2
```

**Das ist die Magie!** Knoten 10 löscht damit die Verbindung zu Knoten 5 und verbindet sein `self.left` stattdessen direkt mit Knoten 2. Knoten 5 wurde einfach komplett umgangen.

Direkt danach führt Knoten 10 das finale `return self` aus und das erste Fenster schließt sich ebenfalls.

---

### Das Endergebnis im Speicher

- **Knoten 10** zeigt mit `left` jetzt direkt auf **Knoten 2**.
- **Knoten 5** hat keinen einzigen Zeiger mehr, der auf ihn verweist. Er schwebt isoliert im Speicher.

Da wir in Python sind, merkt das der **Garbage Collector** sofort, schnappt sich Knoten 5 und löscht ihn endgültig aus dem Arbeitsspeicher.

Die Umleitung steht, der Baum ist sauber repariert und die Sortierung stimmt immer noch (2 < 10).

> delete Note Boot.dev

```python
from typing import Any


class BSTNode:
    def delete(self, val: Any) -> "BSTNode | None":
        if self.val is None:
            return None
        if val < self.val:
            if self.left:
                self.left = self.left.delete(val)
            return self
        if val > self.val:
            if self.right:
                self.right = self.right.delete(val)
            return self
        if self.right is None:
            return self.left
        if self.left is None:
            return self.right
        min_larger_node = self.right
        while min_larger_node.left:
            min_larger_node = min_larger_node.left
        self.val = min_larger_node.val
        self.right = self.right.delete(min_larger_node.val)
        return self

    # don't touch below this line

    def __init__(self, val: Any = None) -> None:
        self.left: "BSTNode | None" = None
        self.right: "BSTNode | None" = None
        self.val = val

    def insert(self, val: Any) -> None:
        if not self.val:
            self.val = val
            return

        if self.val == val:
            return

        if val < self.val:
            if self.left:
                self.left.insert(val)
                return
            self.left = BSTNode(val)
            return

        if self.right:
            self.right.insert(val)
            return
        self.right = BSTNode(val)

    def get_min(self) -> Any:
        current = self
        while current.left is not None:
            current = current.left
        return current.val

    def get_max(self) -> Any:
        current = self
        while current.right is not None:
            current = current.right
        return current.val
```

> delete Node Google

```python
def delete_node(root, key):
    if not root:
        return root # Wert nicht gefunden

    # 1. Den Wert im Baum suchen
    if key < root.val:
        root.left = delete_node(root.left, key)
    elif key > root.val:
        root.right = delete_node(root.right, key)

    # 2. Wert gefunden! Jetzt löschen
    else:
        # Fall 1 & 2: Kein linkes oder kein rechtes Kind
        if not root.left:
            return root.right
        if not root.right:
            return root.left

        # Fall 3: Zwei Kinder
        # Finde den kleinsten Wert auf der rechten Seite (Stellvertreter)
        temp = root.right
        while temp.left:
            temp = temp.left

        # Wert überschreiben
        root.val = temp.val

        # Den doppelten Stellvertreter auf der rechten Seite löschen
        root.right = delete_node(root.right, temp.val)

    return root
```

> Youtube Basics

```Python
# https://www.youtube.com/watch?v=RFjXYPUBMmU&list=PLMz1vLpcJgGDeVDybqQeZ0EewZcRF_1m_&index=4

# 1. Define a Tree

class Node:
    def __init__(self, data):
        self.left = None
        self.right = None
        self.data = data

    def insert(self, data):
        if self.data is None:
            self.data = data

        else:
            if data < self.data:
                if self.left is None:
                    self.left = Node(data)
                else:
                    self.left.insert(data)

            elif data > self.data:
                if self.right is None:
                    self.right = Node(data)
                else:
                    self.right.insert(data)


def inOrderPrint(r):
    if r is None:
        return
    else:
        inOrderPrint(r.left)
        print(r.data, end=" ")
        inOrderPrint(r.right)

def preOrderPrint(r):
    if r is None:
        return
    else:
        print(r.data, end=" ")      # 1. ERST DRUCKEN
        preOrderPrint(r.left)  # 2. DANN LINKS
        preOrderPrint(r.right) # 3. DANN RECHTS

def postOrderPrint(r): # Ein Knoten darf erst dann gedruckt werden, wenn alle seine Kinder und Unterkinder bereits komplett gedruckt wurden
    if r is None:
        return
    else:
        postOrderPrint(r.left)   # 1. ERST LINKS
        postOrderPrint(r.right)  # 2. DANN RECHTS
        print(r.data, end=" ")            # 3. ZUM SCHLUSS DRUCKEN


root = Node("g")
root.insert("c")
root.insert("b")
root.insert("a")
root.insert("e")
root.insert("d")
root.insert("f")
root.insert("i")
root.insert("h")
root.insert("j")
root.insert("k")


inOrderPrint(root)
print()
preOrderPrint(root)
print()
postOrderPrint(root)

"""
               g
             /   \
            c     i
           / \   / \
          b   e h   j
         /   / \     \
        a   d   f     k
"""



```

> Neural Nine

```python
# https://www.youtube.com/watch?v=DlWxqU3LLpY&t=849s

"""
          [ 6 ] (Root)
         /     \
       [2]     [19]
      /   \    /   \
    [1]   [4] [11] [29]
"""

class TreeNode:
    def __init__(self, value):
        self.left = None
        self.right = None
        self.value = value
        self.content = None  # Später im Video hinzugefügt, um Daten zu speichern

    def insert(self, value, content=None):
        if value < self.value:
            if self.left is None:
                self.left = TreeNode(value)
                self.left.content = content
            else:
                self.left.insert(value, content)
        else:
            if self.right is None:
                self.right = TreeNode(value)
                self.right.content = content
            else:
                self.right.insert(value, content)

    def in_order_traversal(self):
        if self.left:
            self.left.in_order_traversal()
        print(self.value)
        if self.right:
            self.right.in_order_traversal()

    def pre_order_traversal(self):
        print(self.value)
        if self.left:
            self.left.pre_order_traversal()
        if self.right:
            self.right.pre_order_traversal()

    def post_order_traversal(self):
        if self.left:
            self.left.post_order_traversal()
        if self.right:
            self.right.post_order_traversal()
        print(self.value)

    def find(self, value):
        if value < self.value:
            if self.left is None:
                return None
            return self.left.find(value)
        elif value > self.value:
            if self.right is None:
                return None
            return self.right.find(value)
        else:
            return self


# --- Beispiel aus dem Video zur Verwendung ---

# Erstellen des Baums mit dem Root-Knoten 6
tree = TreeNode(6)

# Werte einfügen
tree.insert(2)
tree.insert(4)
tree.insert(1)
tree.insert(19, {"data": "hello world"})  # Mit Content-Beispiel
tree.insert(29)
tree.insert(11)

print("--- In-Order Traversal (Sortierte Reihenfolge) ---")
tree.in_order_traversal()

print("\n--- Knoten Suchen ---")
node = tree.find(19)
if node:
    print(f"Gefunden: {node.value} mit Inhalt: {node.content}")
else:
    print("Nicht gefunden")

# ==============================================================================
# MERKZETTEL: WIE DIE REKURSION BEIM EINFÜGEN (INSERT) FUNKTIONIERT
# ==============================================================================
# Beispiel: Wir haben die Root (6) und das linke Child (2). Jetzt rufen wir
# tree.insert(1) auf. Was passiert im Hintergrund?
#
# 1. START AN DER WURZEL:
#    Das Programm startet an der Root (6). Es stellt fest: Die 1 ist kleiner
#    als 6, aber links sitzt schon die 2 (if self.left is None ist also FALSCH).
#
# 2. DAS DURCHREICHEN:
#    Weil links besetzt ist, springt das Programm in den else-Block und reicht
#    die 1 eine Etage tiefer mit dem Befehl: self.left.insert(1)
#
# 3. DER PERSPEKTIVENWECHSEL (Der wichtigste Rekursions-Moment!):
#    Für den nächsten Moment vergisst das Programm die 6 komplett. Der neue
#    Mittelpunkt der Welt ("self") ist ab jetzt der Knoten (2)!
#
# 4. DAS SPIEL STARTET VON VORN (aus Sicht der 2):
#    - Ist value (1) kleiner als self.value (2)? -> JA!
#    - Ist self.left von der 2 aktuell None? -> JA! (Dort ist noch frei!)
#
# 5. DIE LANDUNG:
#    Der Knoten (2) sagt: "Super, dann bin ich ab jetzt dein Elternteil!"
#    und erstellt TreeNode(1) als sein eigenes self.left.
#
# ERKENNTNIS: Die Rekursion sorgt dafür, dass sich das Problem von einer Ebene
# zur nächsten durchreicht, bis es eine freie Stelle (None) findet.
# ==============================================================================

"""
📋 Links: Schau nach links (if self.left) → Wenn da jemand ist, springe hin.
🖨️ Ich: Drucke meinen eigenen Wert (print(self.value)).
📋 Rechts: Schau nach rechts (if self.right) → Wenn da jemand ist, springe hin.
"""
```
