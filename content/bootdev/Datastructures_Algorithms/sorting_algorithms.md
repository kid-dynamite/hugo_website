+++
date = '2026-09-11T18:10:44+02:00'
draft = false
title = 'Sorting Algorithms'
showAuthor = false
weight =10
layout = "simple"
summary = "🚀 Sorting Algorithms"
+++

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
