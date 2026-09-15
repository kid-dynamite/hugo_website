+++
date = '2026-09-14T20:09:53+02:00'
draft = false
title = 'Linked_Lists'
showAuthor = false
weight =12
layout = "simple"
summary = "🚀 Linked_Lists"
+++

> Boot.dev exercise

```python

class Node:
    def __init__(self, val) -> None:
        self.val = val
        self.next: "Node | None" = None

    def set_next(self, node: "Node | None") -> None:
        self.next = node

    def __repr__(self) -> str:
        return self.val


class LinkedList:
    head: Node | None

    def __init__(self) -> None:
        self.head = None

    def __iter__(self):
        # 1. Startpunkt beim Kopf der Liste setzen
        node = self.head

        # 2. Schleife läuft, solange 'node' nicht None ist
        while node is not None:
            # 3. Gib den aktuellen Knoten an den for-Loop weiter
            yield node

            # 4. Springe zum nächsten Knoten weiter
            node = node.next


ll = LinkedList()
ll.head = Node("first")
ll.head.next = Node("second")
ll.head.next.next = Node("third")

for node in ll:
    print(node.val)

# first
# second
# third
```

> Basics

```python
# Nodes
# 1. Value - anything, strings, integers, objects
# 2. The Next Node

class linkedListNode:
    def __init__(self, value, nextNode=None):
        self.value = value
        self.nextNode = nextNode

"""
# "3" --> "7" --> "10"

node1 = linkedListNode("3") # "3"
node2 = linkedListNode("7") # "7"
node3 = linkedListNode("10") # "10"

node1.nextNode = node2 # node1 --> node2 , "3" -> "7"
node2.nextNode = node3 # node2 --> node3 , "7" -> "10"

# node1 --> node2 --> node3


currentNode = node1
while True:
    print(currentNode.value, "->", end=" ")
    if currentNode.nextNode is None:
        print("None")
        break
    currentNode = currentNode.nextNode

# 3 -> 7 -> 10 -> None
"""

class linkedList:
    def __init__(self, head=None):
        self.head = head

    def insert(self, value):
        node = linkedListNode(value)
        if self.head is None:
            self.head = node
            return

        currentNode = self.head
        while True:
            if currentNode.nextNode is None:
                currentNode.nextNode = node
                break
            currentNode = currentNode.nextNode

    def printLinkedList(self):
        currentNode = self.head
        while currentNode is not None:
            print(currentNode.value, "->", end=" ")
            currentNode = currentNode.nextNode
        print("None")


ll = linkedList()
ll.printLinkedList()
ll.insert("3")
ll.printLinkedList()
ll.insert("44")
ll.printLinkedList()
ll.insert("66")
ll.printLinkedList()

```

> Insert at beginning

```python
def prepend(self, value):
    node = linkedListNode(value)

    # 1. Die neue Box zeigt auf den bisherigen Start der Liste
    node.nextNode = self.head

    # 2. Der Start der Liste wird auf die neue Box umgesetzt
    self.head = node
```

> head & tail

```python
class linkedListNode:
    def __init__(self, value, nextNode=None):
        self.value = value
        self.nextNode = nextNode

class UltimateLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    # O(1) - Blitzschnell, da keine Schleife
    def prepend(self, value):
        node = linkedListNode(value)
        node.nextNode = self.head
        self.head = node

        # Sonderfall: Wenn die Liste vorher leer war, ist das neue Element auch das Ende
        if self.tail is None:
            self.tail = node

    # O(1) - Ebenfalls blitzschnell dank des tail-Zeigers!
    def insert_at_end(self, value):
        node = linkedListNode(value)
        if self.head is None:
            self.head = node
            self.tail = node
            return

        self.tail.nextNode = node # Direkt ans Ende anknüpfen
        self.tail = node          # Das neue Ende abspeichern

    def printList(self):
        currentNode = self.head
        while currentNode is not None:
            print(currentNode.value, "->", end=" ")
            currentNode = currentNode.nextNode
        print("None")

# --- Testlauf ---
ll = UltimateLinkedList()

print("--- Am Ende einfügen (mit tail-Optimierung) ---")
ll.insert_at_end("Mitte")
ll.insert_at_end("Hinten")
ll.printList() # Mitte -> Hinten -> None

print("\n--- Vorne einfügen (prepend) ---")
ll.prepend("Vorne")
ll.printList() # Vorne -> Mitte -> Hinten -> None
```
