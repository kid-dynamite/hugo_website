+++
date = '2026-10-11T11:48:22+02:00'
draft = false
title = 'Training exercises'
showAuthor = false
weight =16
layout = "simple"
summary = "🚀 Training exercises"
+++

> Character Count Report - sorting dictionary and tuple-list with sorted-lambda

```python
def generate_character_report(character_counts):
    lst = []
    acc_dict = {}
    header = "Character Report\n----------------\n"
    for key, value in character_counts.items():

        if key.isalpha():
            key_lower = key.lower()
            acc_dict[key_lower] = (acc_dict.get(key_lower, 0) + value)
    if not acc_dict:
        return header + "(no alphabetic characters)"

    for key, value in acc_dict.items():
        lst.append((key, value))

    sorted_lst = sorted(lst, key=lambda x: (-x[1], x[0]))

    for tuple in sorted_lst:
        header += f"{tuple[0]}: {tuple[1]}\n"

    return header.rstrip()




counts_1 = {"!": 4}
counts = {"A": 2, "a": 3, "!": 4, "C": 3, "e": 2, "c": 3, "b": 5,}
print(generate_character_report(counts))


# Multi-Attribute Sorting" oder "Sorting by multiple keys"
```

> MiniStack

```python
class MinStack:
    """
    Ein MinStack ist ein LIFO-Stapel (Last-In, First-Out), der zusätzlich
    in der Lage ist, das aktuell kleinste Element in O(1)-Zeit zurückzugeben.

    DER TRICK:
    Wir nutzen zwei parallele Listen:
    1. self.stack_list: Speichert alle Elemente ganz normal.
    2. self.min_list:   Speichert eine Historie der Minima. Das aktuelle Minimum
                        liegt IMMER ganz oben (am Ende der Liste).
    """

    def __init__(self):
        self.stack_list = []
        self.min_list = []

    def push(self, value: int) -> None:
        """
        Legt ein Element oben auf den Stapel.
        Laufzeit: O(1) - Da Python-Listen am Ende blitzschnell erweitern.
        """
        # 1. Element auf den normalen Stapel legen
        self.stack_list.append(value)

        # 2. Prüfen, ob das neue Element ein neues Minimum ist
        # Fall A: Die Minimum-Liste ist noch komplett leer
        if not self.min_list:
            self.min_list.append(value)
        # Fall B: Das neue Element ist kleiner oder gleich (<=) dem aktuellen Minimum
        elif value <= self.min_list[-1]:
            self.min_list.append(value)

    def pop(self) -> int:
        """
        Entfernt das oberste Element vom Stapel und gibt es zurück.
        Laufzeit: O(1) - Da das Löschen am Ende keine Elemente verschieben muss.
        """
        if not self.stack_list:
            raise IndexError("pop von einem leeren Stapel nicht möglich")

        # 1. Das oberste Element holen und vom Hauptstapel löschen
        value = self.stack_list[-1]
        del self.stack_list[-1]

        # 2. Synchronisation mit der Minimum-Liste:
        # Wenn das gelöschte Element das aktuelle Minimum war,
        # müssen wir es auch aus der Minimum-Historie entfernen.
        if value == self.min_list[-1]:
            del self.min_list[-1]

        return value

    def top(self) -> int:
        """
        Gibt das oberste Element zurück, ohne es zu entfernen.
        Laufzeit: O(1)
        """
        if not self.stack_list:
            raise IndexError("top von einem leeren Stapel nicht möglich")
        return self.stack_list[-1]

    def get_min(self) -> int:
        """
        Gibt das kleinste Element im gesamten Stapel zurück.
        Laufzeit: O(1) - Ein einfacher Blick aufs Ende von self.min_list!
        """
        if not self.min_list:
            raise IndexError("get_min von einem leeren Stapel nicht möglich")
        return self.min_list[-1]


# =====================================================================
# AUSFÜHRLICHES ANWENDUNGSBEISPIEL (Zum Nachvollziehen im Editor)
# =====================================================================
if __name__ == "__main__":
    print("--- 1. MinStack initialisieren ---")
    stack = MinStack()

    print("\n--- 2. Elemente pushen ---")
    # Wir pushen 5
    stack.push(5)
    print(f"Push(5)   -> Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"            Aktuelles Minimum: {stack.get_min()}") # Ausgabe: 5

    # Wir pushen 3 (Neues Minimum!)
    stack.push(3)
    print(f"Push(3)   -> Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"            Aktuelles Minimum: {stack.get_min()}") # Ausgabe: 3

    # Wir pushen 7 (Größer als 3 -> min_list ändert sich nicht)
    stack.push(7)
    print(f"Push(7)   -> Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"            Aktuelles Minimum: {stack.get_min()}") # Ausgabe: 3

    # Wir pushen noch eine 3 (Gleich dem Minimum -> muss wegen '<=' auch in die min_list!)
    stack.push(3)
    print(f"Push(3)   -> Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"            Aktuelles Minimum: {stack.get_min()}") # Ausgabe: 3

    print("\n--- 3. Elemente poppen und Minimum beobachten ---")

    # 1. Pop (Wir nehmen die obere 3 weg)
    popped = stack.pop()
    print(f"Pop() -> Geholter Wert: {popped}")
    print(f"         Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"         Aktuelles Minimum: {stack.get_min()}") # Immer noch 3, wegen der ersten 3!

    # 2. Pop (Wir nehmen die 7 weg -> keine Auswirkung auf das Minimum)
    popped = stack.pop()
    print(f"Pop() -> Geholter Wert: {popped}")
    print(f"         Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"         Aktuelles Minimum: {stack.get_min()}") # Immer noch 3

    # 3. Pop (Wir nehmen die letzte 3 weg -> das Minimum springt zurück auf die 5!)
    popped = stack.pop()
    print(f"Pop() -> Geholter Wert: {popped}")
    print(f"         Normaler Stack: {stack.stack_list} | Min-Historie: {stack.min_list}")
    print(f"         Aktuelles Minimum: {stack.get_min()}") # Ausgabe: 5! Magisch synchronisiert.

```

> Graph Traversals: Shortest Path and DFS Order

```python
def bfs_shortest_path_length(graph, start, goal):
    if start == goal:
        return 0
    if not graph:
        return -1

    visited = [start]  # Startknoten ist sofort besucht
    queue = [(start, 0)]

    while queue:
        current_vertex = queue.pop(0)
        node = current_vertex[0]  # Schnappt sich den Namen (String), z. B. 'A'
        dist = current_vertex[1]  # Schnappt sich die Distanz (Zahl), z. B. 0

        if node == goal:
            return dist

        neighbor = graph[node]
        for city in neighbor:
            if city not in visited:
                visited.append(city)  # SOFORT als besucht markieren!
                queue.append((city, dist + 1))
    return -1





def dfs_traversal(graph, start, visited=None):
    # Wenn wir ganz am Anfang sind, erstellen wir die Liste
    if visited is None:
        visited = []

    # Wir tragen die aktuelle Stadt ein, falls sie neu ist
    if start not in visited:
        visited.append(start)

    # Jetzt klappern wir wie gewohnt die Nachbarn ab
    neighbor = graph[start]
    for city in neighbor:
        if city not in visited:
            dfs_traversal(graph, city, visited)

    return visited




graph = {
    "A": ["B", "C"],
    "B": ["D"],
    "C": ["D"],
    "D": []
}

print(bfs_shortest_path_length(graph, "A", "D"))
# 2  (A -> B -> D or A -> C -> D)

print(bfs_shortest_path_length(graph, "D", "D"))
# 0

print(bfs_shortest_path_length(graph, "D", "A"))
# -1 (no path)

# 2
# 0
# -1



graph = {
    "A": ["B", "C"],
    "B": ["D"],
    "C": ["D"],
    "D": []
}

print(dfs_traversal(graph, "A"))
# One valid DFS order is: ["A", "B", "D", "C"]

"""
andere Rekursion

def bfs_shortest_path_length(graph, start, goal):
    if start == goal:
        return 0
    if not graph:
        return -1

    visited = [start]  # Startknoten ist sofort besucht
    queue = [(start, 0)]

    while queue:
        current_vertex = queue.pop(0)
        node = current_vertex[0]  # Schnappt sich den Namen (String), z. B. 'A'
        dist = current_vertex[1]  # Schnappt sich die Distanz (Zahl), z. B. 0

        if node == goal:
            return dist

        neighbor = graph[node]
        for city in neighbor:
            if city not in visited:
                visited.append(city)  # SOFORT als besucht markieren!
                queue.append((city, dist + 1))
    return -1





def dfs_traversal(graph, start, visited=None):
    # Wenn wir ganz am Anfang sind, erstellen wir die Liste
    if visited is None:
        visited = []

    # Wir tragen die aktuelle Stadt ein, falls sie neu ist
    if start not in visited:
        visited.append(start)

    # Jetzt klappern wir wie gewohnt die Nachbarn ab
    neighbor = graph[start]
    for city in neighbor:
        if city not in visited:
            dfs_traversal(graph, city, visited)

    return visited
"""

```

---

title: "Komplexitätstheorie: Verifikation von TSP und Subset Sum"
date: 2026-10-11
draft: false
description: "Wie man vorgeschlagene Lösungen für das Traveling Salesman Problem und das Subset Sum Problem in Polynomialzeit effizient überprüft."
tags: ["Informatik", "Komplexitätstheorie", "Algorithmen", "P-vs-NP"]

---

In der theoretischen Informatik (speziell in der Klasse **NP**) geht es oft nicht darum, wie schwer es ist, eine Lösung zu _finden_, sondern wie einfach es ist, eine vorgeschlagene Lösung (einen sogenannten _Zertifikats-Kandidaten_) zu **verifizieren**.

Hier ist die verständliche Erklärung, wie ein Verifikationsalgorithmus (Verifier) für das **Traveling Salesman Problem (TSP)** und das **Subset Sum Problem** arbeitet.

---

## Das Grundprinzip: Der Zwei-Schritt-Check

Ein Verifizierer darf niemals selbst nach einer Lösung suchen (das würde zu lange dauern). Er verhält sich wie ein strenger Prüfer und macht bei jeder vorgeschlagenen Lösung immer genau zwei Schritte in **Polynomialzeit (\(O(n^k)\))**:

1. **Die Strukturprüfung:** Passt die Form der Lösung überhaupt zum Problem?
2. **Die Bedingungsprüfung:** Erfüllt die Lösung die mathematischen Vorgaben (z.B. Limit oder Zielwert)?

---

## 1. Traveling Salesman Problem (Problem des Handlungsreisenden)

**Die Ausgangslage:** Du hast eine Liste von Städten, die Distanzen (Gewichte) dazwischen und ein maximales Reisebudget (z.B. _"Du darfst insgesamt nicht mehr als 500 km fahren"_). Jemand behauptet, eine Route gefunden zu haben, und gibt dir eine Liste von Städtenamen.

### So prüft der Verifizierer:

- **Schritt 1 (Struktur):** Er nimmt die Liste und zählt nach. Sind alle Städte genau einmal enthalten? Endet die Route wieder an der Startstadt? (Prüfung, ob es eine gültige _Permutation/Rundreise_ ist).
- **Schritt 2 (Bedingung):** Er schaut in der Graphen-Datenbank nach, wie weit die Städte voneinander entfernt sind, addiert alle Teilstrecken auf und prüft: _Ist die Gesamtsumme \(\le\) Budget?_

---

## 2. Subset Sum Problem (Teilsummenproblem)

**Die Ausgangslage:** Du hast eine Menge von Zahlen (z.B. `[3, 9, 14, 21, 5]`) und einen Zielwert (z.B. `22`). Jemand behauptet, eine Teilmenge dieser Zahlen ergibt exakt 22, und gibt dir als Lösung die Positionen (Indizes) dieser Zahlen im Array.

### So prüft der Verifizierer:

- **Schritt 1 (Struktur):** Er prüft, ob die angegebenen Positionen gültig sind und nicht doppelt verwendet wurden (Prüfung, ob es eine echte _Untermenge_ ist).
- **Schritt 2 (Bedingung):** Er holt die echten Zahlen von diesen Positionen ab, addiert sie zusammen und prüft: _Ist die Summe exakt gleich dem Zielwert?_

---

## Der entscheidende Unterschied (Auf einen Blick)

Mithilfe von Blowfish-Shortcodes lässt sich der Unterschied am besten visualisieren:

{{< badge >}}Reihenfolge & Graph{{< /badge >}} vs. {{< badge >}}Ungeordnete Menge{{< /badge >}}

| Kriterium                 | Traveling Salesman (TSP)                          | Subset Sum                                  |
| :------------------------ | :------------------------------------------------ | :------------------------------------------ |
| **Was wird eingereicht?** | Eine geordnete Sequenz (Pfad)                     | Eine ungeordnete Auswahl (Indizes)          |
| **Struktur-Prüfung**      | Ist es ein geschlossener Kreis durch alle Knoten? | Sind es valide Elemente der Ursprungsmenge? |
| **Mathematik dahinter**   | Nachschlagen von Kanten im Graphen + Addition     | Reine Element-Addition im Array             |

**Zusammenfassend in einem Satz:** Beim TSP musst du die exakte **Reihenfolge** der Schritte beachten und in einem Graphen nachschlagen, während du bei Subset Sum einfach nur Zahlen aus einem Topf nimmst und sie **zusammenzählst**, ohne dass die Reihenfolge eine Rolle spielt. Beide Überprüfungen sind aber extrem schnell (in Polynomialzeit) erledigt!
