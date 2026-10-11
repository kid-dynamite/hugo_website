+++
date = '2026-09-23T12:14:46+02:00'
draft = false
title = 'Hashmaps + Subset Sum Problem'
showAuthor = false
weight =15
layout = "simple"
summary = "🚀 Hashmaps + Subset Sum Problem"
+++

> Subset Sum Problem

```python
def subset_sum(nums: list[int], target: int) -> bool:
    # Wir starten die Suche beim letzten Index der Liste (len(nums) - 1)
    return find_subset_sum(nums, target, len(nums) - 1)


def find_subset_sum(nums: list[int], target: int, index: int) -> bool:
    # Basisfall 1: Wenn das Ziel exakt 0 erreicht hat, haben wir eine Kombination gefunden!
    if target == 0:
        return True

    # Basisfall 2: Wenn der Index unter 0 rutscht und das Ziel nicht 0 ist,
    # haben wir alle Zahlen dieser Kombination erfolglos durchsucht.
    if index < 0 and target != 0:
        return False

    # Fall A: Die aktuelle Zahl ist größer als das restliche Ziel.
    # Wir MÜSSEN sie überspringen (Index um 1 verringern, Ziel bleibt gleich).
    if nums[index] > target:
        return find_subset_sum(nums, target, index - 1)

    # Fall B: Die Zahl würde theoretisch passen. Wir probieren beide Wege aus:
    else:
        # Weg 1: Wir lassen die Zahl weg (Ziel bleibt gleich)
        exclude_current = find_subset_sum(nums, target, index - 1)

        # Weg 2: Wir nehmen die Zahl mit (Ziel verringert sich um den Wert der Zahl)
        include_current = find_subset_sum(nums, target - nums[index], index - 1)

        # Wenn einer der beiden Wege True ergibt, liefert die gesamte Funktion True
        return exclude_current or include_current


# ==========================================
# Werte zum Einsetzen und Ausprobieren:
# ==========================================

# Beispiel 1: Das erfolgreiche Beispiel aus der Erklärung (1 + 5 = 6)
influencer_liste_1 = [1, 2, 5]
ziel_1 = 6
ergebnis_1 = subset_sum(influencer_liste_1, ziel_1)
print(f"Test 1: Finden wir {ziel_1} in {influencer_liste_1}? -> {ergebnis_1}")  # Erwartet: True

# Beispiel 2: Das Gegenbeispiel (Keine Kombination ergibt 6)
influencer_liste_2 = [2, 5]
ziel_2 = 6
ergebnis_2 = subset_sum(influencer_liste_2, ziel_2)
print(f"Test 2: Finden wir {ziel_2} in {influencer_liste_2}? -> {ergebnis_2}")  # Erwartet: False

# Beispiel 3: Ein etwas größeres Rätsel (2 + 3 + 5 = 10)
influencer_liste_3 = [2, 3, 5, 8]
ziel_3 = 10
ergebnis_3 = subset_sum(influencer_liste_3, ziel_3)
print(f"Test 3: Finden wir {ziel_3} in {influencer_liste_3}? -> {ergebnis_3}")  # Erwartet: True
```

> Linear Probing

```python
from typing import Any


class HashMap:
    def insert(self, key: str, value: Any) -> None:
        index = self.key_to_index(key)
        original_index = index
        first_iteration = True

        # Schleife läuft, solange der Slot belegt ist UND der Key nicht übereinstimmt
        while self.hashmap[index] is not None and self.hashmap[index][0] != key:
            if not first_iteration and index == original_index:
                raise Exception("hashmap is full")

            # Zum nächsten Index springen und am Ende wieder von vorne anfangen (Modulo)
            index = (index + 1) % len(self.hashmap)
            first_iteration = False

        # Leeren Slot oder passenden Key gefunden -> Einfügen/Überschreiben
        self.hashmap[index] = (key, value)

    def get(self, key: str) -> Any:
        index = self.key_to_index(key)
        original_index = index
        first_iteration = True

        # Schleife läuft, solange der Slot belegt (nicht None) ist
        while self.hashmap[index] is not None:
            if self.hashmap[index][0] == key:
                return self.hashmap[index][1]

            if not first_iteration and index == original_index:
                raise Exception("sorry, key not found")

            # Zum nächsten Index springen und am Ende wieder von vorne anfangen
            index = (index + 1) % len(self.hashmap)
            first_iteration = False

        # Wenn wir auf ein None stoßen, existiert der Key in der Map nicht
        raise Exception("sorry, key not found")

    # don't touch below this line

    def __init__(self, size: int) -> None:
        self.hashmap: list[tuple[str, Any] | None] = [None for i in range(size)]

    def key_to_index(self, key: str) -> int:
        total = 0
        for c in key:
            total += ord(c)
        return total % len(self.hashmap)

    def __repr__(self) -> str:
        final = ""
        for i, v in enumerate(self.hashmap):
            if v is not None:
                final += f" - {v!s}\n"
        return final


```
