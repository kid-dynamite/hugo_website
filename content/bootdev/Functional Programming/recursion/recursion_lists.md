+++
date = '2026-07-26T22:58:22+02:00'
draft = false
title = 'Recursion_lists'
showAuthor = false
weight =52
layout = "simple"
summary = "🚀 list recursion"
+++

```python

# return the sum of a list

def sum_nums(nums: list[int]) -> int:
    if len(nums) == 0:
        return 0
    return nums[0] + sum_nums(nums[1:])


print(sum_nums([1, 2, 3, 4, 5]))        # prints: 15
```

> Find Max in nested list

```python
def maxList(lst):
    """
    Nutzt eine Schleife für die Hauptebene und Rekursion für verschachtelte Listen.
    Berücksichtigt ausschließlich int-Werte.
    """
    tempMax = float("-inf")

    for item in lst:
        if isinstance(item, int):
            if item > tempMax:
                tempMax = item

        if isinstance(item, list):
            result = maxList(item)
            if result > tempMax:
                tempMax = result

    return tempMax if tempMax != float("-inf") else None


# Testlauf
print(maxList([9, 31, 9, "string", [99, 4.5, "er"],500])) # Ausgabe: 99 (4.5 wird ignoriert)

```

> Flatten list with for-loop

```python
def flatten_ints(lst):
    """
    Sammelt alle int-Werte aus einer verschachtelten Liste in einer flachen Liste.
    """
    res = []

    for x in lst:
        if isinstance(x, int):
            res.append(x)
        elif isinstance(x, list):
            sub_res = flatten_ints(x)
            res.extend(sub_res)

    return res


# Testlauf
daten = [9, 31, 9, "string", [99, 4.5, "er"], 500]
print(flatten_ints(daten))
# Ausgabe: [9, 31, 9, 99, 500]


```

> Increment ints by 1 in nested list

```python
def inc_nested(lst):
    """
    Erhöht alle int-Werte in einer verschachtelten Liste um 1.
    """
    res = []

    for x in lst:
        if isinstance(x, int):
            res.append(x + 1)

        elif isinstance(x, list):
            sub_res = inc_nested(x)
            res.append(sub_res)

        else:
            res.append(x)

    return res


# Testlauf
daten = [9, 31, 9, "string", [99, 4.5, "er"], 500]
print(inc_nested(daten))
# Ausgabe: [10, 32, 10, 'string', [100, 4.5, 'er'], 501]
```

```python
def maxList(lst):
    """
    Use Recursion to Return the maximum value in the list
    """
    if len(lst) == 0:
        return None
    else:
        tempMax = float("-inf")
        for item in lst:
            # Das if-Statement wird bei JEDEM Element als Erstes abgefragt:
            print(f"--> Abfrage 'if': Ist '{item}' eine Liste?")

            if isinstance(item, list):
                print(f"    JA! '{item}' ist eine Liste. Ich springe tiefer hinein...\n")
                max_in_nested = maxList(item)

                print(f"    <-- Zurück aus der Unterliste. Gefundenes Max dort war: {max_in_nested}")
                if max_in_nested is not None and max_in_nested > tempMax:

                    tempMax = max_in_nested

            elif isinstance(item, int):
                print(f"    NEIN! Aber '{item}' ist eine Zahl. Vergleiche mit tempMax...")
                if item > tempMax:
                    tempMax = item
                    print(f"    => Neues tempMax ist: {tempMax}")

        return tempMax if tempMax != float("-inf") else None

# Starte den Testlauf
print("=== START DES PROGRAMMS ===")
ergebnis = maxList([9, 31, 9, "string", [99, [201, [501]], 4, "er"]])
print("===========================")
print(f"Endergebnis: {ergebnis}")
```

> Calc the sum of all int i a list

```python


def sum_list_ints(lst):
    add_int = 0
    for item in lst:
        if isinstance(item, list):
            # Rekursiver Aufruf für verschachtelte Listen
            add_int += sum_list_ints(item)
        elif isinstance(item, int) and not isinstance(item, bool):
            # Da bool ein Untertyp von int ist, schließt dies True/False aus
            add_int += item

    return add_int

print(sum_list_ints([9, 31, 9, "string", [99, 4, "er"]]))  # Ausgabe: 152
```

> Return depth of a list with for-loop and recursion

```python
def maxDepth(lst):
    if not isinstance(lst, list):
        return 0

    current_max = 1  # Die aktuelle Liste selbst zählt als Ebene 1

    for item in lst:
        if isinstance(item, list):
            # Hier wird das +1 direkt für die nächste Ebene addiert
            sub_depth = 1 + maxDepth(item)
            if sub_depth > current_max:
                current_max = sub_depth

    return current_max

```

> another way

```python
def maxDepth(lst):
    """
    Nutzt Rekursion, um die maximale Tiefe einer Liste zu berechnen.
    Eine flache Liste wie [1, 2, 3] hat die Tiefe 1.
    """
    if not isinstance(lst, list):
        return 0

    max_sub_depth = 0
    for item in lst:
        if isinstance(item, list):
            sub_depth = maxDepth(item)
            if sub_depth > max_sub_depth:
                max_sub_depth = sub_depth

    return 1 + max_sub_depth

# Test mit Ihrem Beispiel
print(maxDepth([9, 31, 9, "string", [99, 4, "er"]])) # Gibt 2 aus
print(maxDepth([1, [2, [3, [4]]]]))                  # Gibt 4 aus

```

> with for-loop and recursion

```python
def remove_none_values(data):
    # Basis-Fall: Falls die Liste komplett leer ist
    if not data:
        return []

    result = []

    # Die Schleife läuft flach durch die aktuelle Ebene
    for item in data:
        if isinstance(item, list):
            # REKURSION: Wenn es eine Liste ist, tauchen wir ab,
            # reinigen sie und hängen die gereinigte Liste an result an.
            cleaned_nested = remove_none_values(item)
            result.append(cleaned_nested)
        elif item is not None:
            # Normale Werte (int, str etc.) werden einfach behalten
            result.append(item)
        # Wenn item None ist, springt die Schleife weiter (wird ignoriert)

    return result

# TESTFÄLLE
list_1 = [1, None, 3]
print(remove_none_values(list_1))
# Ausgabe: [1, 3]

list_2 = [1, [2, None, 3], None, [None, [4, 5]]]
print(remove_none_values(list_2))
# Ausgabe: [1, [2, 3], [[4, 5]]]
```

> Create a new list with all the strings

```python
def extract_all_text(dom_tree):
    result = []

    for item in dom_tree:
        if isinstance(item, str):
            result.append(item)
        if isinstance(item, list):
            scan_list = extract_all_text(item)
            result.extend(scan_list)


    return result

# TESTFÄLLE

# Ein einfaches Dokument: Eine Überschrift und ein Absatz
site_1 = ["Willkommen", "Auf meiner Website", ["Hier ist Text"]]
print(extract_all_text(site_1))
# Erwartete Ausgabe: ['Willkommen', 'Auf meiner Website', 'Hier ist Text']

# Ein komplexes Dokument mit Zahlen (IDs) und tiefen Verschachtelungen
site_2 = ["Startseite", 1024, ["Menü", ["Home", "Über uns"]], "Footer-Text", 404]
print(extract_all_text(site_2))
# Erwartete Ausgabe: ['Startseite', 'Menü', 'Home', 'Über uns', 'Footer-Text']

```

> Calculate the sum of all ints in a nested list

```python
def calculate_disk_space(folder):
    total_size = 0

    for item in folder:
        if isinstance(item, int):
            total_size += item
        if isinstance(item, list):
            sum_list = calculate_disk_space(item)
            total_size += sum_list





    return total_size

# TESTFÄLLE

# Ein einfacher Ordner mit 3 Dateien
folder_1 = [12, 45, 5]
print(calculate_disk_space(folder_1))
# Erwartete Ausgabe: 62

# Ein verschachtelter Ordner (Hauptordner hat Dateien und zwei Unterordner)
folder_2 = [10, [20, 30], 5, [1, [2, 3]]]
print(calculate_disk_space(folder_2))
# Erwartete Ausgabe: 71
```

```python
def extract_all_text(dom_tree, depth=0):  # depth startet standardmäßig bei 0
    result = []

    for item in dom_tree:
        if isinstance(item, str):
            str_tuple = (item, depth)  # Nutzt die aktuelle Tiefe
            result.append(str_tuple)
        elif isinstance(item, list):
            # Wir tauchen ab und sagen der tieferen Ebene: "Du bist eine Stufe tiefer (+1)!"
            scan = extract_all_text(item, depth + 1)
            result.extend(scan)

    return result


# TESTFÄLLE (Die Ausgabe liefert dir jetzt den Text UND die exakte HTML-Tiefe!)

site_1 = ["Willkommen", "Auf meiner Website", ["Hier ist Text"]]
print(extract_all_text(site_1))
# Ausgabe: [('Willkommen', 0), ('Auf meiner Website', 0), ('Hier ist Text', 1)]

site_2 = ["Startseite", 1024, ["Menü", ["Home", "Über uns"]], "Footer-Text", 404]
print(extract_all_text(site_2))
# Ausgabe: [('Startseite', 0), ('Menü', 1), ('Home', 2), ('Über uns', 2), ('Footer-Text', 0)]
```

```python
def render_dom_text(dom_tree, depth=0):
    # HIER KOMMT DEIN CODE HIN
    # Nutze Rekursion und Schleifen, um die Strings zu finden.
    # Tipp: Ein Zeilenumbruch in Python ist "\n"

    my_str = ""

    for item in dom_tree:
        if isinstance(item, str):
            leerzeichen = " " * depth
            my_str += leerzeichen + item + "\n"

        if isinstance(item, list):
            scan = render_dom_text(item, depth + 2)

            my_str +=  scan
    return my_str


# TESTFALL
site_2 = ["Startseite", 1024, ["Menü", ["Home", "Über uns"]], "Footer-Text", 404]

# Das Ergebnis soll ein einziger, langer String mit Zeilenumbrüchen sein
print(render_dom_text(site_2))
```

```python
def clean_comments(post_data):
    cleaned = []

    # HIER KOMMT DEIN CODE HIN
    # Nutze eine for-Schleife, um durch post_data zu gehen.
    # Wenn es ein String ist: Prüfe, ob er NICHT "DELETED" ist, und füge ihn hinzu.
    # Wenn es eine Liste (Unterkommentare) ist: Tauche per Rekursion ab!

    for item in post_data:
        if isinstance(item, str) and item != "DELETED":
            cleaned.append(item)
        if isinstance(item, list):
            scan = clean_comments(item)
            cleaned.extend(scan)


    return cleaned


# TESTDATEN (Ein Post mit Kommentaren und tiefen Unterkommentaren)
social_post = [
    "Tolles Foto!",
    "DELETED",
    ["Ich stimme zu!", "DELETED", ["Sehe ich auch so!", "Absolut!"]],
    "Schöne Grüße!",
    ["Ganz nett", "DELETED"]
]

# TESTAUFRUF
print(clean_comments(social_post))
```

```python
def find_and_count(lst, target):
    # Initialisiere deine Variablen (Akkumulatoren)
    found = False
    count = 0

    """
        Nutze eine for-Schleife.
        Wenn ein Element eine Liste ist -> rekursiver Aufruf.
        Wenn ein Element direkt das 'target' ist -> Zähler erhöhen und found auf True setzen.
    """
    for item in lst:
        if item == target:
            found = True
            count += 1
        elif isinstance(item, list):
            sub_found, sub_count = find_and_count(item, target)
            found = found or sub_found
            count += sub_count
            #print(result)


    return (found, count)


# --- TESTCASES ---
# Test 1: Normaler Treffer
nested_1 = [1, 2, "such mich", [3, "such mich", [4]], "such mich"]
print(find_and_count(nested_1, "such mich"))
# Erwartete Ausgabe: (True, 3)

# Test 2: Element existiert nicht
nested_2 = [1, 2, [3, 4]]
print(find_and_count(nested_2, 99))
# Erwartete Ausgabe: (False, 0)

```

> Count depth of a list

```python
def get_depth(lst):
    """
    Ermittelt die maximale Tiefe einer verschachtelten Liste mittels Rekursion.
    Eine flache Liste hat die Tiefe 1.
    """
    # Basisfall: Wenn es keine Liste ist, hat es keine Tiefe
    if not isinstance(lst, list):
        return 0

    max_sub_depth = 0

    # Schleife durchläuft die aktuelle Ebene
    for x in lst:
        if isinstance(x, list):
            # Rekursiver Aufruf: Wie tief ist diese Unterliste?
            sub_depth = get_depth(x)

            # Merke dir die tiefste gefundene Unterliste
            if sub_depth > max_sub_depth:
                max_sub_depth = sub_depth

    # 1 für die aktuelle Ebene plus die Tiefe der tiefsten Unterliste
    return 1 + max_sub_depth


# --- Testläufe ---

# Ebene 1: Eine normale flache Liste
print(get_depth([1, 2, 3]))
# Ausgabe: 1

# Ebene 2: Eine Liste in einer Liste
print(get_depth([1, [2, 3], 4]))
# Ausgabe: 2

# Ebene 3: Drei Ebenen tief verschachtelt
print(get_depth([9, 31, ["string", [99, "er"]], 500]))
# Ausgabe: 3

# Ebene 4: Noch eine Ebene tiefer eingebettet
print(get_depth([1, [2, [3, [4]]]]))
# Ausgabe: 4

"""
get_depth([ [ [ ] ] ])       --> Wartet auf Ebene 2... erhält 2. Rechnet 1 + 2. Return 3! (ENDE)
   └── get_depth([ [ ] ])    --> Wartet auf Ebene 3... erhält 1. Rechnet 1 + 1. Return 2!
          └── get_depth([ ]) --> Tiefste Ebene! Keine Unterliste. Rechnet 1 + 0. Return 1!
"""
```

---

title: "Die visuelle Funktionsweise der Rekursion"
date: 2026-08-21
draft: false
description: "Eine schrittweise Erklärung, wie der Call-Stack bei verschachtelten Listen im Speicher arbeitet."

---

{{< lead >}}
Stellen Sie sich jeden Funktionsaufruf wie eine eigene, isolierte Box vor. Jede Box hat ihr eigenes `lst` und ihr eigenes `max_sub_depth`.
{{< /lead >}}

## Die 3 isolierten Boxen im Speicher

### Schritt 1: Der Hinweg (In die Tiefe gehen)

- **Box 1 (Ebene 1) startet:**
  - `lst` = `[1, [2,]]`
  - `max_sub_depth` = **0**
  - Die Schleife sieht `1` (Zahl → ignoriert).
  - Die Schleife sieht `[2,]` (Liste → **Pause!** Rufe Box 2 auf).

- **Box 2 (Ebene 2) startet:**
  - `lst` = `[2,]`
  - Ein _neues_ `max_sub_depth` = **0** wird erzeugt.
  - Die Schleife sieht `2` (Zahl → ignoriert).
  - Die Schleife sieht `` (Liste → **Pause!** Rufe Box 3 auf).

- **Box 3 (Ebene 3) startet:**
  - `lst` = ``
  - Ein _drittes_ `max_sub_depth` = **0** wird erzeugt.
  - Die Schleife sieht `3` (Zahl → ignoriert).
  - Die Schleife ist fertig. `max_sub_depth` in Box 3 blieb **0**.

---

### Schritt 2: Der Rückweg (Die Rückrechnung)

Jetzt schließt sich der Kreis von unten nach oben. Hier passiert die entscheidende Veränderung im Speicher:

1. **Box 3 rechnet und schließt:**
   - Box 3 rechnet: `1 + max_sub_depth` → `1 + 0 = 1`.
   - Sie schickt `1` zurück an Box 2 und wird gelöscht.

2. **Box 2 wacht auf und verändert sich:**
   - Box 2 empfängt `sub_depth = 1` von Box 3.
   - Sie prüft: Ist `sub_depth (1) > max_sub_depth (0)`? **Ja!**
   - **Hier ändert es sich:** Das `max_sub_depth` von Box 2 wird auf **1** gesetzt.
   - Die Schleife von Box 2 ist fertig.
   - Box 2 rechnet: `1 + max_sub_depth` → `1 + 1 = 2`.
   - Sie schickt `2` zurück an Box 1 und wird gelöscht.

3. **Box 1 wacht auf und verändert sich:**
   - Box 1 empfängt `sub_depth = 2` von Box 2.
   - Sie prüft: Ist `sub_depth (2) > max_sub_depth (0)`? **Ja!**
   - **Hier ändert es sich wieder:** Das `max_sub_depth` von Box 1 wird auf **2** gesetzt.
   - Die Schleife von Box 1 ist fertig.
   - Box 1 rechnet das Endergebnis: `1 + max_sub_depth` → `1 + 2 = 3`.

::: { .font-bold .text-xl .text-primary-500 .mt-4 }
Endergebnis: 3
:::

```python
class Node:
    def __init__(self, name, is_folder=False, size=0):
        self.name = name
        self.is_folder = is_folder  # True wenn Ordner, False wenn Datei
        self.size = size            # Größe in MB (nur relevant, wenn is_folder=False)
        self.children = []          # Liste von anderen Node-Objekten (nur wenn is_folder=True)

    def add_child(self, child_node):
        self.children.append(child_node)

    def get_total_size(self):
        # Wenn der aktuelle Node eine Datei ist, ist die Größe einfach self.size
        if not self.is_folder:
            return self.size

        # Wenn es ein Ordner ist, müssen wir die Größen aller Kinder zusammenrechnen
        total_size = 0

        # HIER DEINE LOGIK REINSCHREIBEN
        # Schleife durch self.children...
        # Rekursiver Aufruf für jedes Kind...

        return total_size


# --- TESTCASE (Der Verzeichnisbaum) ---
# Wir bauen eine Struktur:
# root_ordner/
#   ├── foto.jpg (5 MB)
#   └── projekte_ordner/
#         ├── text.txt (2 MB)
#         └── code.py (3 MB)

root = Node("root_ordner", is_folder=True)
foto = Node("foto.jpg", is_folder=False, size=5)

projekte = Node("projekte_ordner", is_folder=True)
text = Node("text.txt", is_folder=False, size=2)
code = Node("code.py", is_folder=False, size=3)

# Baum zusammenbauen
projekte.add_child(text)
projekte.add_child(code)

root.add_child(foto)
root.add_child(projekte)

# Test-Aufruf
print(root.get_total_size())
# ERWARTETE AUSGABE: 10
# (Erklärung: 5MB vom Foto + 2MB Text + 3MB Code = 10MB)
```
