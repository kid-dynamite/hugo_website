+++
date = '2026-07-27T21:22:24+02:00'
draft = false
title = 'Recursion_dic_depth'
showAuthor = false
weight =54
layout = "simple"
summary = "🚀 recursion on depth of a dictionary"
+++

```python
mein_dict = {
    "a": 1,
    "b": 2,
    "c": 3,
    "d": 4,
    "e": 5
}

print(mein_dict)


wert = mein_dict["a"]

wert = mein_dict.get("a")
print(wert)
```

> code without comments...

```python
def dict_depth(d, max_depth_so_far):
    if not isinstance(d, dict) or not d:
        return max_depth_so_far

    current_max = max_depth_so_far

    for v in d.values():
        depth_of_subdict = dict_depth(v, max_depth_so_far + 1)

        if depth_of_subdict > current_max:
            current_max = depth_of_subdict

    return current_max

d = {
    "melee_weapons": {
        "stabbies": {
            "spears": 4,
            "knives": 3,
        },
        "bows": 6
    }
}

ergebnis = dict_depth(d, 0)
print(f"Die maximale Tiefe beträgt: {ergebnis}")
```

> code with comments...

```python
def dict_depth(d, max_depth_so_far):
    # SCHRITT 1: Die Abbruchbedingung (Base Case)
    # Wenn 'd' kein Dictionary mehr ist (z.B. eine Zahl) ODER das Dictionary leer ist...
    if not isinstance(d, dict) or not d:
        # ...dann stoppen wir hier und geben die bisher erreichte Tiefe zurück.
        return max_depth_so_far

    # SCHRITT 2: Startwert für die aktuelle Ebene setzen
    # Wir nehmen an, die maximale Tiefe ist erst einmal das, was wir mitgebracht haben.
    current_max = max_depth_so_far

    # SCHRITT 3: Alle Inhalte durchsuchen
    # Wir gehen nacheinander jeden Wert ('v') im aktuellen Dictionary durch.
    for v in d.values():
        # REKURSION: Wir rufen die Funktion selbst für den Wert 'v' auf.
        # Da wir eine Ebene tiefer gehen, erhöhen wir 'max_depth_so_far' um +1.
        depth_of_subdict = dict_depth(v, max_depth_so_far + 1)

        # SCHRITT 4: Rekord prüfen
        # Wenn dieser Pfad tiefer war als alles, was wir auf dieser Ebene bisher gesehen haben...
        if depth_of_subdict > current_max:
            # ...dann ist das unser neuer Höchstwert für diese Ebene.
            current_max = depth_of_subdict

    # SCHRITT 5: Ergebnis zurückgeben
    # Wenn alle Werte geprüft wurden, geben wir den finalen Höchstwert zurück.
    return current_max


# ==========================================
# BEISPIEL ZUM AUSPROBIEREN (aus dem Video)
# ==========================================

d = {
    "melee_weapons": {          # Ebene 1
        "stabbies": {          # Ebene 2
            "spears": 4,       # Ebene 3 (Inhalt ist eine Zahl -> hier stoppt es)
            "knives": 3,       # Ebene 3
        },
        "bows": 6              # Ebene 2 (Inhalt ist eine Zahl -> hier stoppt es)
    }
}

# Wir starten den Aufruf mit dem Dictionary und einer Starttiefe von 1.
ergebnis = dict_depth(d, 0)

print(f"Die maximale Tiefe beträgt: {ergebnis}")  # Ausgabe: 3
```

> Recursion on a tree

```python
def list_files(
    parent_directory: dict[str, dict | None], current_filepath: str = ""
) -> list[str]:
    empty_list = []
    for key,value in parent_directory.items():
        new_path = current_filepath + "/" + key
        if value == None:
            empty_list.append(new_path)
        else:
            empty_list.extend(list_files(value, new_path))
    return empty_list

"""
---------------------------------
Input: {'Documents': {'Proposal.docx': None, 'Report': {'AnnualReport.pdf': None, 'Financials.xlsx': None}}, 'Downloads': {'picture1.jpg': None, 'picture2.jpg': None}}
Expected:
    /Documents/Proposal.docx
    /Documents/Report/AnnualReport.pdf
    /Documents/Report/Financials.xlsx
    /Downloads/picture1.jpg
    /Downloads/picture2.jpg
Actual:
    /Documents/Proposal.docx
    /Documents/Report/AnnualReport.pdf
    /Documents/Report/Financials.xlsx
    /Downloads/picture1.jpg
    /Downloads/picture2.jpg
Pass
============= PASS ==============
1 passed, 0 failed, 3 skipped
"""
```
