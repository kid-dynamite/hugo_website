+++
date = '2026-08-01T10:20:38+02:00'
draft = false
title = 'Recursion_dict_exercises'
showAuthor = false
weight =56
layout = "simple"
summary = "🚀 dictionary recursion exercises"
+++

> Max depth of a dictionary

```python
def dict_depth(d, max_depth_so_far):


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

> Recursion on a Tree

```python
def list_files(
    parent_directory: dict[str, dict | None], current_filepath: str = ""
) -> list[str]:


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

"""
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

> another one

```python
def find_key_recursive(data, target_key):
if not isinstance(data, dict):
return None
if target_key in data:
return data[target_key]
for value in data.values():
result = find_key_recursive(value, target_key)
if result is not None:
return result
return None
```
