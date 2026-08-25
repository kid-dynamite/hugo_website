+++
date = '2026-08-24T15:22:14+02:00'
draft = false
title = 'Recursion_boot.dev_exercises'
showAuthor = false
weight =66
layout = "simple"
summary = "🚀 original boot.dev exercises"
+++

> Factorial

```python
def factorial_r(x: int) -> int:
    if x == 0:
        return 1

    return x * factorial_r(x-1)

print(factorial_r(5)) # 120
```

> Zipmap

```python
def zipmap(keys: list[str], values: list[float]) -> dict[str, float]:

    if not keys or not values:
        return {}

    result = zipmap(keys[1:], values[1:])

    result[keys[0]]= values[0]

    return result


# Liste 1: Die Schlüssel (Keys)
test_keys = ["Python", "JavaScript", "Go", "Rust"]

# Liste 2: Die Werte (Values)
test_values = [4.8, 4.2, 4.6, 4.9]

print(zipmap(test_keys, test_values))
```

---

title: "Visualisierung von Rekursion in Python"
date: 2026-08-24
draft: false
description: "Wie funktioniert Rekursion? Ein praktisches Beispiel mit visualisierten Funktionsaufrufen im Terminal."
tags: ["Python", "Programmierung", "Rekursion"]
categories: ["Tutorials"]

---

{{< lead >}}
Rekursion zu verstehen kann am Anfang knifflig sein. Am besten sieht man, was im Hintergrund passiert, wenn man den Ablauf im Terminal Schritt für Schritt visualisiert.
{{< /lead >}}

Hier ist die angepasste `zipmap`-Funktion, die mithilfe von visuellen Einrückungen (`indent`) und Emojis genau zeigt, wie Python in die Rekursion "eintaucht" und danach die Daten beim "Auftauchen" zusammensetzt.

## Der Python-Code

```python
def zipmap(keys: list[str], values: list[float], depth: int = 0) -> dict[str, float]:
    # Erstellt visuelle Einrückung für das Terminal
    indent = "  " * depth

    print(f"{indent}➔ Aufruf zipmap(keys={keys}, values={values})")

    # Basisfall
    if not keys or not values:
        print(f"{indent}  ⚠️ Basisfall erreicht! Gebe leeres Dict {{}} zurück.")
        return {}

    # Rekursiver Aufruf nach unten
    result = zipmap(keys[1:], values[1:], depth + 1)

    # Zusammensetzen auf dem Rückweg nach oben
    print(f"{indent}   Letztes result vor dem Einfügen: {result}")
    print(f"{indent}  ➕ Füge hinzu: {keys[0]} -> {values[0]}")

    result[keys[0]] = values[0]

    print(f"{indent}✔ Rückgabe von dieser Ebene: {result}")
    return result

# Testdaten
test_keys = ["Python", "JavaScript", "Go", "Rust"]
test_values = [4.8, 4.2, 4.6, 4.9]

print("\n--- START DER REKURSION ---\n")
final_result = zipmap(test_keys, test_values)
print("\n--- ENDERGEBNIS ---")
print(final_result)
```

## Live-Ablauf im Terminal

Hier ist die exakte Terminal-Ausgabe. Achte darauf, wie sich die Einrückung wie eine Treppe nach rechts aufbaut und beim Zurückgeben wieder nach links wandert:

```text
--- START DER REKURSION ---

➔ Aufruf zipmap(keys=['Python', 'JavaScript', 'Go', 'Rust'], values=[4.8, 4.2, 4.6, 4.9])
  ➔ Aufruf zipmap(keys=['JavaScript', 'Go', 'Rust'], values=[4.2, 4.6, 4.9])
    ➔ Aufruf zipmap(keys=['Go', 'Rust'], values=[4.6, 4.9])
      ➔ Aufruf zipmap(keys=['Rust'], values=[4.9])
        ➔ Aufruf zipmap(keys=[], values=[])
          ⚠️ Basisfall erreicht! Gebe leeres Dict {} zurück.
        Letztes result vor dem Einfügen: {}
        ➕ Füge hinzu: Rust -> 4.9
      ✔ Rückgabe von dieser Ebene: {'Rust': 4.9}
     Letztes result vor dem Einfügen: {'Rust': 4.9}
     ➕ Füge hinzu: Go -> 4.6
    ✔ Rückgabe von dieser Ebene: {'Rust': 4.9, 'Go': 4.6}
   Letztes result vor dem Einfügen: {'Rust': 4.9, 'Go': 4.6}
   ➕ Füge hinzu: JavaScript -> 4.2
  ✔ Rückgabe von dieser Ebene: {'Rust': 4.9, 'Go': 4.6, 'JavaScript': 4.2}
 Letztes result vor dem Einfügen: {'Rust': 4.9, 'Go': 4.6, 'JavaScript': 4.2}
 ➕ Füge hinzu: Python -> 4.8
✔ Rückgabe von dieser Ebene: {'Rust': 4.9, 'Go': 4.6, 'JavaScript': 4.2, 'Python': 4.8}

--- ENDERGEBNIS ---
{'Rust': 4.9, 'Go': 4.6, 'JavaScript': 4.2, 'Python': 4.8}
```

{{< alert icon="lightbulb" cardColor="#e0f2fe" iconColor="#0369a1" >}}
**Gut zu wissen:** Der Code wandert erst komplett nach rechts (Aufrufe), bis die Listen leer sind. Danach arbeitet er sich wieder nach links (Rückgaben) und befüllt das Dictionary von hinten nach vorne!
{{< /alert >}}

> Sum nested list

```python
def sum_nested_list(lst: list[int | list]) -> int:

    total: int= 0

    for item in lst:

        if isinstance(item, int):
            total += item

        elif isinstance(item, list):
            result = sum_nested_list(item)
            total += result

    return total



root: list[int | list] = [5, [6, 7], [[8, 9], 10]]
print(sum_nested_list(root))
# 45

```

> Recursion on a tree

```python
def list_files(
    parent_directory: dict[str, dict | None], current_filepath: str = ""
) -> list[str]:

    path_list = []

    for key, value in parent_directory.items():

        new_path = current_filepath + "/" + key

        if value == None:
            path_list.append(new_path)

        if isinstance(value, dict):
            result = list_files(value, new_path)
            #print(result)
            path_list.extend(result)


    return path_list



parent_directory: dict[str, dict | None] = {
    "Documents": {
        "Proposal.docx": None,
        "Receipts": {
            "January": {"receipt1.txt": None, "receipt2.txt": None},
            "February": {"receipt3.txt": None},
        },
    },
}


file_paths: list[str] = [
    "/Documents/Proposal.docx",
    "/Documents/Receipts/January/receipt1.txt",
    "/Documents/Receipts/January/receipt2.txt",
    "/Documents/Receipts/February/receipt3.txt",
]

print(list_files(parent_directory))

```

> Count nested levels

```python
def count_nested_levels(
    nested_documents: dict[int, dict], target_document_id: int, level: int = 1
) -> int:

    for key, value in nested_documents.items():

        if key == target_document_id:
            return level

        if isinstance(value, dict):
            result = count_nested_levels(value, target_document_id, level + 1)
            if result is not None:
                return result


    return None





nested_documents: dict[int, dict] = {1: {3: {}}, 2: {}}
print(count_nested_levels(nested_documents, 3))

```
