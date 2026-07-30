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

```python
def dict_depth(d, max_depth_so_far):
    """
    SCHRITT-FÜR-SCHRITT-ERKLÄRUNG DER PRINT-AUSGABEN:

    1. Start auf Ebene 0 (d):
       - Die Funktion startet mit dem Haupt-Dictionary.
       - max_depth_so_far ist 0. current_max wird 0.
       - Die Schleife geht zum ersten Wert: dem Dictionary unter "melee_weapons".

    2. Abstieg zu Ebene 1 ("melee_weapons"):
       - Rekursiver Aufruf mit max_depth_so_far = 1. current_max wird 1.
       - Die Schleife geht zum ersten Wert: dem Dictionary unter "stabbies".

    3. Abstieg zu Ebene 2 ("stabbies"):
       - Rekursiver Aufruf mit max_depth_so_far = 2. current_max wird 2.
       - Die Schleife nimmt den ersten Wert: "spears": 4.

    4. Abstieg zu Ebene 3 ("spears"):
       - Rekursiver Aufruf für den Wert 4 mit max_depth_so_far = 3.
       - Da 4 kein Dictionary ist (if not isinstance), wird sofort 3 zurückgegeben.

    5. Zurück auf Ebene 2 (Verarbeitung von "spears"):
       - depth_of_subdict erhält den Wert 3.
       - AUSGABE 1: depth: 3
       - Da 3 > 2 (current_max), wird current_max auf 3 gesetzt.
       - AUSGABE 2: current_max: 3
       - Die Schleife geht zum nächsten Wert: "knives": 3.

    6. Abstieg zu Ebene 3 ("knives"):
       - Rekursiver Aufruf für den Wert 3 mit max_depth_so_far = 3.
       - Da 3 kein Dictionary ist, wird sofort 3 zurückgegeben.

    7. Zurück auf Ebene 2 (Verarbeitung von "knives"):
       - depth_of_subdict erhält den Wert 3.
       - AUSGABE 3: depth: 3
       - Da 3 nicht größer als das aktuelle current_max (3) ist, bleibt current_max unverändert.
       - AUSGABE 4: current_max: 3
       - Das "stabbies"-Dictionary ist fertig. Ebene 2 gibt ihr current_max (3) an Ebene 1 zurück.

    8. Zurück auf Ebene 1 (Verarbeitung von "stabbies"):
       - depth_of_subdict erhält den Rückgabewert 3.
       - AUSGABE 5: depth: 3
       - Da 3 > 1 (current_max), wird current_max auf 3 gesetzt.
       - AUSGABE 6: current_max: 3
       - Die Schleife auf Ebene 1 geht zum nächsten Wert: "bows": 6.

    9. Abstieg zu Ebene 2 ("bows"):
       - Rekursiver Aufruf für den Wert 6 mit max_depth_so_far = 2.
       - Da 6 kein Dictionary ist, wird sofort 2 zurückgegeben.

    10. Zurück auf Ebene 1 (Verarbeitung von "bows"):
        - depth_of_subdict erhält den Wert 2.
        - AUSGABE 7: depth: 2
        - Da 2 nicht größer als current_max (3) ist, bleibt es bei 3.
        - AUSGABE 8: current_max: 3
        - Ebene 1 ist fertig und gibt ihr current_max (3) an Ebene 0 zurück.

    11. Zurück auf Ebene 0 (Haupt-Ebene):
        - depth_of_subdict erhält den Wert 3.
        - AUSGABE 9: depth: 3
        - Da 3 > 0 (current_max), wird current_max auf 3 gesetzt.
        - AUSGABE 10: current_max: 3
        - Die Funktion endet und liefert das Endergebnis.
    """
    if not isinstance(d, dict) or not d:
        return max_depth_so_far

    current_max = max_depth_so_far
    for v in d.values():
        depth_of_subdict = dict_depth(v, max_depth_so_far + 1)
        print(f"depth: {depth_of_subdict}")
        if depth_of_subdict > current_max:
            current_max = depth_of_subdict
        print(f"current_max: {current_max}")

    return current_max


# Test-Daten aus der boot.dev Aufgabe
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

> Exercise

```python
def calculate_total_size(parent_directory: dict) -> int:
    total_size = 0
    # DEIN CODE HIER:
    # 1. Gehe mit einer Schleife durch das Dictionary (items).
    # 2. Wenn der Value ein int ist (die Dateigröße), addiere ihn zu total_size.
    # 3. Wenn der Value ein dict ist (ein Unterordner), rufe die Funktion rekursiv auf
    #    und addiere das Ergebnis zu total_size.
    return total_size


# --- TESTDATEN ZUM AUSPROBIEREN ---
file_system = {
    "Documents": {
        "Proposal.docx": 4,  # 4 MB
        "Report": {
            "AnnualReport.pdf": 15,  # 15 MB
            "Financials.xlsx": 8,  # 8 MB
        },
    },
    "Downloads": {
        "picture1.jpg": 3,  # 3 MB
        "picture2.jpg": 5,  # 5 MB
    },
}

# Aufruf der Funktion
ergebnis = calculate_total_size(file_system)
print(f"Gesamtgröße: {ergebnis} MB")

# Erwartetes Ergebnis: 4 + 15 + 8 + 3 + 5 = 35 MB
```

> Solution

```python
def calculate_total_size(parent_directory: dict) -> int:
    total_size = 0

    for key, value in parent_directory.items():
        if isinstance(value, int):
            # Wenn es eine Zahl ist, direkt addieren
            total_size += value
        elif isinstance(value, dict):
            # Wenn es ein Unterordner (dict) ist, tiefer eintauchen
            total_size += calculate_total_size(value)

    return total_size
```

> Exercise

```python
def find_large_files(parent_directory: dict, min_size: int = 5) -> list[str]:
    large_files = []
    # DEIN CODE HIER:
    # 1. Gehe mit einer Schleife durch das Dictionary.
    # 2. Wenn der Value ein int ist UND größer als min_size ist:
    #    Füge den Dateinamen (Key) zu large_files hinzu.
    # 3. Wenn der Value ein dict ist:
    #    Tauche per Rekursion unter und hänge die Ergebnisse an large_files an.
    return large_files


# --- TESTDATEN ZUM AUSPROBIEREN ---
file_system = {
    "Projects": {
        "presentation.pptx": 12,  # > 5 -> Treffer!
        "notes.txt": 1,
        "SourceCode": {
            "main.py": 2,
            "database.db": 45,  # > 5 -> Treffer!
        },
    },
    "Music": {
        "song1.mp3": 4,
        "podcast.mp3": 60,  # > 5 -> Treffer!
    },
}

# Aufruf der Funktion (sucht standardmäßig nach Dateien > 5 MB)
ergebnis = find_large_files(file_system)
print(f"Große Dateien: {ergebnis}")

# Erwartetes Ergebnis: ['presentation.pptx', 'database.db', 'podcast.mp3']
# (Die Reihenfolge in der Liste kann je nach Ablauf variieren)
```

> Solution

```python
def find_large_files(parent_directory: dict, min_size: int = 5) -> list[str]:
    large_files = []

    for key, value in parent_directory.items():
        if isinstance(value, int):
            # Prüfen, ob die Datei die Mindestgröße überschreitet
            if value > min_size:
                large_files.append(key)
        elif isinstance(value, dict):
            # Rekursiver Aufruf für Unterordner
            sub_results = find_large_files(value, min_size)
            large_files.extend(sub_results)

    return large_files
```

> Exercise

```python
def find_and_format_large_files(
    parent_directory: dict, current_path: str = "", min_size: int = 10
) -> list[str]:
    results = []
    # DEIN CODE HIER:
    # 1. Schleife über das Dictionary.
    # 2. Berechne den neuen Pfad (wie in Aufgabe 1).
    # 3. Wenn es ein int (Datei) ist UND > min_size:
    #    Füge den Text f"{neuer_pfad} ({value}MB)" zu results hinzu.
    # 4. Wenn es ein dict (Ordner) ist: Rekursion und .extend()
    return results


# --- TESTDATEN ---
file_system = {
    "Videos": {
        "tutorial.mp4": 45,  # Treffer! (>10)
        "intro.mp4": 5,
    },
    "Backup": {
        "2026": {
            "database.sql": 120,  # Treffer! (>10)
            "config.json": 1,
        }
    },
}

# Aufruf
ergebnis = find_and_format_large_files(file_system)
print(ergebnis)

# Erwartetes Ergebnis exakt so:
# [
#     '/Videos/tutorial.mp4 (45MB)',
#     '/Backup/2026/database.sql (120MB)'
# ]
```

> Solution

```python
def find_and_format_large_files(
    parent_directory: dict, current_path: str = "", min_size: int = 10
) -> list[str]:
    results = []

    for key, value in parent_directory.items():
        # Pfad für die aktuelle Ebene zusammenbauen
        new_path = current_path + "/" + key

        if isinstance(value, int):
            # Filtern und im gewünschten Textformat speichern
            if value > min_size:
                results.append(f"{new_path} ({value}MB)")

        elif isinstance(value, dict):
            # Rekursiver Aufruf mit dem neuen Pfad
            sub_results = find_and_format_large_files(
                value, new_path, min_size
            )
            results.extend(sub_results)

    return results
```
