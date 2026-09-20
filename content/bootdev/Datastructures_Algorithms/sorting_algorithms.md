+++
date = '2026-09-11T18:10:44+02:00'
draft = false
title = 'Sorting Algorithms'
showAuthor = false
weight =10
layout = "simple"
summary = "🚀 Sorting Algorithms"
+++

> Fibonacci-iteration

```python
def fib(n: int) -> int:
    a, b = 0, 1

    for _ in range(n):
        a, b = b, a + b  # Der magische Schritt

    return a

print(fib(1))

"""

    [ a ]    [ b ]
      │        │
      ▼        ▼
    [ b ]  [ a + b ]


"""
```

> Fibonacci-Iteration

```python
def fib(n: int) -> int:
    if n<=1:
        return n

    a = 0
    b = 1

    for i in range(1, n):

        a, b = b, a+b


    return b

```

> Quick Sort

```python
def quick_sort(nums: list[int], low: int, high: int) -> None:
    if low<high:
        p = partition(nums, low, high)
        quick_sort(nums, low, p-1)
        quick_sort(nums, p +1,high)



def partition(nums: list[int], low: int, high: int) -> int:
    i = low-1
    pivot = nums[high]

    for j in range(low, high):
        if nums[j] < pivot:
            i += 1
            nums[j],nums[i] = nums[i], nums[j]
    nums[i+1], nums[high] = nums[high], nums[i+1]
    return i+1
```

Die **Big-O-Laufzeit (Zeitkomplexität)** für Quick Sort hängt stark davon ab, wie gut das gewählte Pivot-Element das Array aufteilt.

Hier ist die Übersicht der drei Szenarien:

| Szenario                            | Zeitkomplexität (Big O) | Wann tritt es auf?                                                                                                                                                                                 |
| :---------------------------------- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Best Case** (Bester Fall)         | O(n log n)              | Das Pivot-Element teilt das Array jedes Mal in **zwei exakt gleich große Hälften**.                                                                                                                |
| **Average Case** (Durchschnitt)     | O(n log n)              | Das Pivot-Element teilt das Array in **zufällige Proportionen** (selbst bei einer 90:10-Aufteilung bleibt es mathematisch in dieser Klasse).                                                       |
| **Worst Case** (Schlechtester Fall) | O(n²)                   | Das Pivot-Element ist unglücklich gewählt und ist immer das **kleinste oder größte Element** (z. B. wenn das Array bereits perfekt sortiert ist und man immer das letzte Element als Pivot wählt). |

### 🛠️ Zusätzlicher Faktor: Platzkomplexität (Space Complexity)

Quick Sort sortiert _in-place_ (direkt im Speicher des Arrays). Die Speicherkomplexität liegt bei **O(log n)** im Durchschnitt und **O(n)** im Worst Case. Dieser Speicher wird ausschließlich für den **rekursiven Call-Stack** (Aufrufe-Stapel) benötigt.

> Merge Sort

```python
def merge_sort(nums: list[int]) -> list[int]:
    if len(nums) < 2:
        return nums
    middle = len(nums) // 2
    left = merge_sort(nums[:middle])
    right = merge_sort(nums[middle:])
    return merge(left, right)


def merge(first: list[int], second: list[int]) -> list[int]:
    final = []
    i = 0
    j = 0
    while i < len(first) and j < len(second):
        if first[i] <= second[j]:
            final.append(first[i])
            i += 1
        else:
            final.append(second[j])
            j += 1
    while i < len(first):
        final.append(first[i])
        i += 1
    while j < len(second):
        final.append(second[j])
        j += 1
    return final


    lst = [38, 27, 43, 3, 82, 10, 34, 56,87]

print(merge_sort(lst))


"""
first = [3, 27, 38, 43]
second = [10, 34, 56, 82, 87]
"""


merge_sort(lst)

                  [38, 27, 43, 3, 82, 10, 34, 56, 87]            <-- Start
                     /                           \
         [38, 27, 43, 3]                    [82, 10, 34, 56, 87]
           /         \                        /              \
       [38, 27]    [43, 3]                [82, 10]        [34, 56, 87]
        /    \      /    \                 /    \          /      \
      [38]  [27]  [43]   [3]             [82]  [10]      [34]   [56, 87]

       |     |     |      |               |     |         |      /    \
       |     |     |      |               |     |         |    [56]  [87]
========================================= BASE CASES =========================================
       \     /     \      /               \     /         |      \    /
       [27, 38]    [3, 43]                [10, 82]        |      [56, 87]
           \          /                       \           /         /
           [3, 27, 38, 43]                    [10, 34, 56, 82, 87] /
                  \                                      /
                   [3, 10, 27, 34, 38, 43, 56, 82, 87]           <-- Fertig!

```

### 1. Die Time Complexity (Zeitkomplexität)

Die Zeitkomplexität beschreibt, wie die Laufzeit des Algorithmus in Abhängigkeit zur Anzahl der Eingabeelemente n wächst.

- **Best Case:** O(n log n)
- **Average Case:** O(n log n)
- **Worst Case:** O(n log n)

#### 💡 Das Wichtigste für Boot.dev

Merge Sort verhält sich in seiner Laufzeit **vollkommen konstant**. Es spielt absolut keine Rolle, ob die übergebene Liste bereits perfekt vorsortiert ist, sich in exakt umgekehrter Reihenfolge befindet oder komplett zufällig zusammengewürfelt wurde – Merge Sort teilt und verschmilzt die Arrays in jedem einzelnen Szenario auf die exakt gleiche Weise.

#### 🔍 Warum genau n log n?

Die mathematische Laufzeit setzt sich aus zwei Kernoperationen zusammen, die multipliziert werden:

- **Das Aufteilen (Der Logarithmus log n):** Bei jedem rekursiven Schritt wird die Liste halbiert. Die Anzahl der Ebenen (die Baumtiefe), bis wir bei Einzelelementen der Länge 1 ankommen, entspricht genau log2(n) Schritten.
- **Das Zusammenfügen (Die lineare Komponente n):** Auf jeder einzelnen Ebene des Rekursionsbaums müssen die Elemente beim Zusammenführen (`merge`) miteinander verglichen werden. Da sich das Programm dabei jedes Element genau einmal ansieht, benötigt diese Operation auf jeder Ebene eine Laufzeit von n.

Zusammengesetzt und multipliziert ergibt dies die fundamentale Gesamtlaufzeit von **O(n log n)**.

---

### 2. Die Space Complexity (Platzkomplexität)

Die Platzkomplexität gibt an, wie viel zusätzlichen Arbeitsspeicher der Algorithmus während der Ausführung benötigt.

- **Space Complexity:** O(n)

#### 🔍 Warum benötigt Merge Sort so viel Speicher?

Merge Sort ist **kein In-Place-Algorithmus**. Wie du direkt im Python-Code sehen kannst, werden durch die Slicing-Operationen `nums[:middle]` und `nums[middle:]` sowie durch die Definition des Arrays `final = []` in der `merge`-Hilfsfunktion kontinuierlich neue, temporäre Teillisten im Speicher erzeugt.

Im schlimmsten Fall (auf der obersten Ebene beim Zusammenfügen der beiden letzten großen Hälften) benötigt das Programm temporär noch einmal exakt genauso viel Speicherplatz, wie die originale Liste lang ist. Daher skaliert der zusätzliche Speicherbedarf linear mit n.

---

### 3. Vergleich mit Quick Sort (Beliebte Boot.dev-Frage)

In Systemarchitektur-Interviews und den Boot.dev-Aufgaben werden Merge Sort und Quick Sort standardmäßig gegenübergestellt, da sie unterschiedliche Trade-offs eingehen:

- **Stabilität im Worst Case:** Merge Sort garantiert selbst im absolut schlimmsten Fall eine stabile Laufzeit von O(n log n). Quick Sort hingegen kann bei einer ungünstigen Wahl des Pivot-Elements (z. B. bei bereits sortierten Listen) im Worst Case auf eine Laufzeit von O(n²) abstürzen.
- **Speichereffizienz:** Hier gewinnt Quick Sort deutlich. Da Quick Sort ein _In-Place_-Verfahren ist und Elemente direkt innerhalb des originalen Arrays vertauscht, benötigt es lediglich einen minimalen Hilfsspeicher von O(log n) für den rekursiven Aufrufstapel. Merge Sort hingegen verbraucht durch seine temporären Kopien den vollen zusätzlichen Speicher von O(n).

---

### 4. Merge Sort ist "stabil" (Stable)

Die Stabilität ist eine fundamentale Eigenschaft von Sortieralgorithmen, wenn Datensätze mit identischen Schlüsseln verarbeitet werden.

#### 🔍 Was bedeutet Stabilität in der Praxis?

Wenn deine unsortierte Liste Duplikate enthält – beispielsweise zwei Mal die Zahl 5 – sorgt die logische Bedingung `if first[i] <= second[j]:` in deiner `merge`-Funktion dafür, dass das Element aus der linken Teilliste zuerst genommen wird.

Das bedeutet: Diejenige 5, die im ursprünglichen, unsortierten Array weiter links stand, wird auch im finalen, sortierten Array garantiert weiter links platziert. Die relative Reihenfolge von identischen Elementen bleibt somit perfekt erhalten, was vor allem beim Sortieren von komplexeren Objekten (z. B. Benutzerdaten erst nach Nachname, dann nach Vorname) absolut essenziell ist.

> Bubble Sort

```python
def bubble_sort(nums: list[int]) -> list[int]:
    swapping = True
    end = len(nums)
    while swapping:
        swapping = False
        for i in range(1, end):
            if nums[i - 1] > nums[i]:
                temp = nums[i - 1]
                nums[i - 1] = nums[i]
                nums[i] = temp
                swapping = True
        end -= 1
    return nums
```

## Die Big-O-Komplexität von Bubble Sort (Optimiert)

Bei dieser optimierten Variante von Bubble Sort – die sowohl eine `swapping`-Variable (Early Exit) als auch ein dynamisch verkürztes Ende (`end -= 1`) nutzt – unterscheidet sich die Laufzeit je nach Zustand der Liste drastisch.

### Komplexitäts-Übersicht

| Szenario                                  | Zeitkomplexität | Warum?                                                                                                                                                                                                                             |
| :---------------------------------------- | :-------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Best Case** (Perfekt sortiert)          | **O(n)**        | Dank `swapping`: Die Schleife läuft exakt einmal durch (n-1 Schritte). Da nichts getauscht wird, bricht der Algorithmus sofort ab.                                                                                                 |
| **Average Case** (Zufällig gemischt)      | **O(n²)**       | Die Anzahl der Vergleiche und Verschiebungen verhält sich im Durchschnitt proportional zur quadrierten Anzahl der Elemente.                                                                                                        |
| **Worst Case** (Exakt rückwärts sortiert) | **O(n²)**       | Dank `end -= 1` wird die innere Schleife pro Runde kürzer (n-1, n-2, n-3...). Mathematisch halbiert das die Schritte auf n \* (n-1) / 2. In Big-O fällt die Konstante 1/2 weg, in der Praxis spart es aber **50% der Vergleiche**! |
| **Space Complexity** (Platzbedarf)        | **O(1)**        | Der Algorithmus arbeitet _in-place_. Er benötigt unabhängig von der Listengröße keinen zusätzlichen Speicherplatz.                                                                                                                 |

---

### Wichtige Klarstellung: Quadratisch vs. Exponentiell

Oft wird ein O(n²) Verhalten fälschlicherweise als "exponentiell" bezeichnet. In der Informatik gibt es hier jedoch einen gewaltigen Unterschied:

- **Quadratisch (O(n²)):** Die Variable n steht an der Basis. Verdoppelt sich die Liste (2n), vervierfacht sich die Laufzeit (2² = 4). Das ist das Verhalten von Bubble Sort im Worst Case.
- **Exponentiell (O(2ⁿ)):** Die Variable n steht im Exponenten. Jedes einzelne zusätzliche Element verdoppelt die Laufzeit (n+1 führt zu doppelter Zeit). Das wäre um Welten langsamer als Bubble Sort.

### Warum dein `end -= 1` in der Praxis den Unterschied macht

Obwohl sowohl der Standard-Bubble-Sort als auch diese optimierte Version im Worst Case mit **O(n²)** klassifiziert werden, ignoriert die Big-O-Notation konstante Faktoren.

In der echten Welt führt der Code durch das kontinuierliche Vorziehen des Endes **nur halb so viele Operationen** aus wie die unoptimierte Lehrbuch-Variante. Er ist somit exakt doppelt so schnell.

> Binary Search

````python
def binary_search(target: int, arr: list[int]) -> bool:
    low = 0
    high = len(arr) - 1

    while low <= high:
        median = (low+high) // 2
        if arr[median] == target:
            return True
        elif arr[median] > target:
            high = median - 1
        else:
            low = median + 1
    return False
    ```
````

## Die Big-O-Komplexität von Binary Search (Iterativ)

Die Binärsuche (Binary Search) gehört zu den effizientesten Suchalgorithmen überhaupt. Da sich die verbleibende Datenmenge bei jedem einzelnen Schritt halbiert, arbeitet der Algorithmus mit einer extrem flachen, logarithmischen Laufzeitkurve.

### Komplexitäts-Übersicht

| Szenario                                              | Zeitkomplexität | Warum?                                                                                                                                                     |
| :---------------------------------------------------- | :-------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Best Case** (Direkter Treffer)                      | **O(1)**        | Volltreffer: Das gesuchte Element liegt exakt in der Mitte der Liste. Die Schleife findet es direkt im allerersten Durchlauf.                              |
| **Average Case** (Der Durchschnitt)                   | **O(log n)**    | Im Schnitt halbiert der Algorithmus den Suchbereich so lange, bis das gesuchte Element gefunden wird.                                                      |
| **Worst Case** (Das Element fehlt oder liegt am Ende) | **O(log n)**    | Selbst im absolut schlimmsten Fall (das Element existiert nicht), braucht die Schleife bei 1 Million Elementen **nur maximal 20 Schritte** für Gewissheit. |
| **Space Complexity** (Platzbedarf)                    | **O(1)**        | Da eine iterative `while`-Schleife verwendet wird, bleibt der Speicherbedarf konstant. Es werden nur die Pointer (low, high, median) verschoben.           |

---

### Was bedeutet O(log n) in der Praxis?

Logarithmisches Wachstum ist nach O(1) der Traum jedes Entwicklers. Während sich bei einem quadratischen Algorithmus wie Bubble Sort (O(n²)) die Laufzeit bei einer Verdopplung der Liste vervierfacht, steigt der Aufwand bei der Binärsuche **um genau einen einzigen Schritt** an!

- Eine Liste mit **1.024** Elementen benötigt maximal **10 Schritte** (2¹⁰ = 1.024).
- Eine Liste mit **über 1 Milliarde** Elementen benötigt maximal **nur 30 Schritte** (2³⁰ ≈ 1,07 Milliarden).

### Wichtiger Hinweis zu Iterativ vs. Rekursiv

Dieser O(1) Speicherbedarf gilt exklusiv für die **iterative Variante mit einer `while`-Schleife**.

Würde man die Binärsuche stattdessen rekursiv lösen, müsste das System für jeden Schritt einen neuen Funktionsaufruf auf den Call Stack legen. Der Speicherbedarf würde dadurch auf **O(log n)** ansteigen, weshalb die iterative Lösung in der Praxis und besonders in Python immer die performantere und sicherere Wahl ist.
