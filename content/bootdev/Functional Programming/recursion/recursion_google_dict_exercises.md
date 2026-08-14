+++
date = '2026-08-13T11:36:30+02:00'
draft = false
title = 'Recursion_google_dict_exercises'
showAuthor = false
weight =65
layout = "simple"
summary = "🚀 simple google  exercises on dictionary recursion"
+++

> gibt die Tiefe eines gegebenen Keys (Target_document) aus

```python
def count_nested_levels(
    nested_documents: dict[int, dict], target_document_id: int, level: int = 1
) -> int:
    for keys, values in nested_documents.items():
        if keys == target_document_id:
            return level

        # OPTIMIERUNG: Nur tiefer gehen, wenn das Dictionary nicht leer ist
        if values:
            result = count_nested_levels(values, target_document_id, level + 1)
            if result != -1:
                return result

    return -1

nested_documents : dict[int, dict] = {1: {3: {}}, 2: {}}

print(count_nested_levels( nested_documents, 3))


"""
    "Rekursion in Bäumen (Dictionaries): Wenn der tiefere Aufruf -1
    zurückgibt, war das eine Sackgasse. Das if result != -1 sorgt dafür,
    dass wir die Sackgasse ignorieren und in der Schleife des aktuellen
    Raums weiter nach dem nächsten Weg suchen."
"""
```

> Count all Keys in a dictionary

```python
def count_all_keys(nested_dict: dict) -> int:
    total_keys = 0

    for keys, values in nested_dict.items():
        total_keys += 1

        if type(values) == dict:
            result = count_all_keys(values)
            total_keys += result

    # HIER DEINEN CODE EINFÜGEN
    # Tipp: Nutze eine for-Schleife.
    # Prüfe mit type(values) == dict, ob du tiefer gehen musst.

    return total_keys


# --- TESTBEREICH ---
# Dieses Dictionary hat insgesamt 6 Schlüssel:
# 'user', 'profile', 'name', 'level', 'gear', 'weapon'
test_data = {
    "user": "Disciple",
    "profile": {
        "name": "Sharpshooter",
        "level": 46
    },
    "gear": {
        "weapon": "Bow"
    }
}

# Aufruf der Funktion
ergebnis = count_all_keys(test_data)

print(f"Dein Ergebnis: {ergebnis}")
print(f"Richtig wäre:  6")
```

> Suche nach einem Value und return True or False if in dictionary

```python
def find_word_in_dict(nested_dict: dict, target_word: str) -> bool:

    for keys, values in nested_dict.items():
        if values == target_word:
            return True
        if type(values) == dict:
            result = find_word_in_dict(values, target_word)

            if result:
                return result




        # 1. Schritt: Prüfe, ob das aktuelle 'values' genau unser 'target_word' ist.
        #    Wenn ja, was musst du tun?

        # 2. Schritt: Prüfe, ob 'values' wieder ein Dictionary ist.
        #    Wenn ja, starte den rekursiven Aufruf.
        #    ACHTUNG: Nutze hier deine Intuition aus der Boot.dev-Aufgabe!
        #    Reiche das Ergebnis nur nach oben weiter, wenn es 'True' ist!


    return False


# --- TESTBEREICH ---
test_firma = {
    "name": "Max",
    "info": {
        "stadt": "Berlin",
        "status": "aktiv"
    },
    "gear": {
        "setup": {
            "monitore": "zwei",
            "tastatur": "mechanisch"
        }
    }
}

# Test 1: Sollte True ergeben
ergebnis_1 = find_word_in_dict(test_firma, "Berlin")
print(f"Suche nach 'Berlin':   Dein Ergebnis: {ergebnis_1} | Richtig wäre: True")

# Test 2: Sollte True ergeben (sitzt ganz tief im Baum!)
ergebnis_2 = find_word_in_dict(test_firma, "mechanisch")
print(f"Suche nach 'mechanisch': Dein Ergebnis: {ergebnis_2} | Richtig wäre: True")

# Test 3: Sollte False ergeben
ergebnis_3 = find_word_in_dict(test_firma, "München")
print(f"Suche nach 'München':  Dein Ergebnis: {ergebnis_3} | Richtig wäre: False")

```

> Sum all the Values in a dictionary

```python
def calculate_total_salary(nested_company: dict) -> int:
    total_salary = 0

    for value in nested_company.values():
        if isinstance(value, int):
            total_salary += value
        if isinstance(value, dict):
            scan_dict = calculate_total_salary(value)
            total_salary += scan_dict


    return total_salary


# --- TESTBEREICH ---
# Diese Firma hat insgesamt ein Gehaltsvolumen von 25.000 €
test_firma = {
    "Chef": 5000,
    "Marketing": {
        "Manager": 4000,
        "Praktikant": 1000
    },
    "IT": {
        "Lead": 6000,
        "Devs": {
            "Senior": 5500,
            "Junior": 3500
        }
    }
}

ergebnis = calculate_total_salary(test_firma)

print(f"Dein Ergebnis: {ergebnis}")
print(f"Richtig wäre:  25000")

```

> Find a specific string in a dict/values and save to a list

```python
def find_secrets(config):
    result = []

    for value in config.values():
        if isinstance(value, str) and value.startswith("secret_"):
            result.append(value)
        if isinstance (value, dict):
            scan_dict = find_secrets(value)
            result.extend(scan_dict)
        if isinstance (value, list):
            for item in value:
                if isinstance(item, str) and item.startswith("secret_"):
                    result.append(item)


    return result

# TESTFALL (Eine typische API-Konfiguration)
app_config = {
    "app_name": "MyAwesomeApp",
    "version": 1.2,
    "auth": {
        "admin_key": "secret_admin123",
        "public_key": "pub_key_xyz",
        "backup_tokens": ["token_1", "secret_backup_999"]
    },
    "database": {
        "host": "localhost",
        "password": "secret_db_password_777"
    }
}

print(find_secrets(app_config))
# Erwartete Ausgabe: ['secret_admin123', 'secret_backup_999', 'secret_db_password_777']
# (Die Reihenfolge in der Liste ist egal)

```
