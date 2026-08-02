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

> Dictionary depth

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

```python
=== START DER REKURSION ===

→ Starte dict_depth mit Wert: {'melee_weapons': {'stabbies': {'spears': 4, 'knives': 3}, 'bows': 6}} (max_depth_so_far=0)
  Startwert auf dieser Ebene: current_max = 0
  • Prüfe Schlüssel 'melee_weapons'...
  → Starte dict_depth mit Wert: {'stabbies': {'spears': 4, 'knives': 3}, 'bows': 6} (max_depth_so_far=1)
    Startwert auf dieser Ebene: current_max = 1
    • Prüfe Schlüssel 'stabbies'...
    → Starte dict_depth mit Wert: {'spears': 4, 'knives': 3} (max_depth_so_far=2)
      Startwert auf dieser Ebene: current_max = 2
      • Prüfe Schlüssel 'spears'...
      → Starte dict_depth mit Wert: 4 (max_depth_so_far=3)
        [OBERES RETURN] Kein Dict! Gebe 3 zurück.
      • Zurück auf Ebene 2! depth_of_subdict wurde geladen mit: 3
        Vergleich: 3 > 2 ist WAHR!
        current_max wurde aktualisiert auf: 3
      • Prüfe Schlüssel 'knives'...
      → Starte dict_depth mit Wert: 3 (max_depth_so_far=3)
        [OBERES RETURN] Kein Dict! Gebe 3 zurück.
      • Zurück auf Ebene 2! depth_of_subdict wurde geladen mit: 3
        Vergleich: 3 > 3 ist FALSCH. current_max bleibt: 3
      [UNTERES RETURN] Schleife fertig! Gebe current_max=3 an die obere Ebene ab.
    • Zurück auf Ebene 1! depth_of_subdict wurde geladen mit: 3
      Vergleich: 3 > 1 ist WAHR!
      current_max wurde aktualisiert auf: 3
    • Prüfe Schlüssel 'bows'...
    → Starte dict_depth mit Wert: 6 (max_depth_so_far=2)
      [OBERES RETURN] Kein Dict! Gebe 2 zurück.
    • Zurück auf Ebene 1! depth_of_subdict wurde geladen mit: 2
      Vergleich: 2 > 3 ist FALSCH. current_max bleibt: 3
    [UNTERES RETURN] Schleife fertig! Gebe current_max=3 an die obere Ebene ab.
  • Zurück auf Ebene 0! depth_of_subdict wurde geladen mit: 3
    Vergleich: 3 > 0 ist WAHR!
    current_max wurde aktualisiert auf: 3
  [UNTERES RETURN] Schleife fertig! Gebe current_max=3 an die obere Ebene ab.

=== ENDE DER REKURSION ===
Die maximale Tiefe beträgt: 3

```

> Dictionary depth - code with comments...

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

> Dictionary depth - code with comments...

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

> Recursion on a tree

```python
def list_files(
    parent_directory: dict[str, dict | None], current_filepath: str = ""
) -> list[str]:
    empty_list = []
    for key,value in parent_directory.items():
        new_path = current_filepath + "/" + key
        if value is None:
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
