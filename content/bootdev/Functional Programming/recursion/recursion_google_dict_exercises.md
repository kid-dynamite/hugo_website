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

```python
def find_files(system):
    files = []

    # HIER KOMMT DEIN CODE HIN
    # Nutze: for key, value in system.items():
    # Wenn der value ein String ist: Datei gefunden -> ab in die Liste damit!
    # Wenn der value ein Dictionary ist: Unterordner gefunden -> Rekursion!

    for value in system.values():
        if isinstance(value, str):
            files.append(value)
        if isinstance(value, dict):

            scan = find_files(value)
            files.extend(scan)

    return files


# TESTDATEN (Ein verschachteltes Dateisystem)
file_system = {
    "rezept.txt": "Pfannkuchen",
    "Bilder": {
        "urlaub.jpg": "Strandfoto",
        "Party": {
            "geburtstag.png": "Kuchenbild"
        }
    },
    "todo.md": "Einkaufen gehen",
    "Dokumente": {
        "vertrag.pdf": "Mietvertrag"
    }
}

# TESTAUFRUF
print(find_files(file_system))
```

```python
def build_sitemap(tree, current_path=""):
    paths = []

    # HIER KOMMT DEIN CODE HIN
    # Nutze: for key, value in tree.items():
    #
    # Tipp 1: Der aktuelle Pfad für dieses Element ist current_path + "/" + key
    # (Wenn current_path noch leer ist, ist es einfach nur key)
    #
    # Tipp 2: Wenn der value ein String ist -> Pfad ist fertig, ab in die files-Liste!
    # Tipp 3: Wenn der value ein dict ist -> Unterordner! Tauche per Rekursion ab
    #         und übergib den neu gebauten Pfad an die nächste Ebene.

    for key, value in tree.items():

        new_path = current_path + key + "/"

        if isinstance(value, str):

            paths.append(new_path)

        if isinstance(value, dict):

            scan = build_sitemap(value, new_path)
            paths.extend(scan)



    return paths


# TESTDATEN (Die Struktur einer Website)
website_structure = {
    "index.html": "Startseite",
    "about.html": "Über uns",
    "blog": {
        "welcome.html": "Erster Post",
        "tech": {
            "python-tips.html": "Rekursions-Guide",
            "index.html": "Blog-Übersicht Tech"
        }
    },
    "contact.html": "Kontaktformular"
}

# TESTAUFRUF
print(build_sitemap(website_structure))

[
    'index.html',
    'about.html',
    'blog/welcome.html',
    'blog/tech/python-tips.html',
    'blog/tech/index.html',
    'contact.html'
]
```

````python
def find_path_in_dict(nested_dict: dict, target_word: str) -> list or None:
    path = []

    for key, value in nested_dict.items():

        if isinstance(value, dict):
            result = find_path_in_dict(value, target_word)

            if result is not None:
                return [key] + result

        elif value == target_word:
            path.append(key)
            return path

    # Wenn das Wort in diesem gesamten (Unter-)Dictionary nicht existiert
    return None
also wir, da info ein dict ist rekursive in die funktion.
da stadt : berlin ist gibt elif value == target_word: den pfad
an result = weiter - richtig?


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

# Test 1: Sollte ['info', 'stadt'] ergeben
pfad_1 = find_path_in_dict(test_firma, "Berlin")
print(f"Pfad zu 'Berlin':     Dein Ergebnis: {pfad_1} | Richtig wäre: ['info', 'stadt']")

# Test 2: Sollte ['gear', 'setup', 'tastatur'] ergeben
pfad_2 = find_path_in_dict(test_firma, "mechanisch")
print(f"Pfad zu 'mechanisch': Dein Ergebnis: {pfad_2} | Richtig wäre: ['gear', 'setup', 'tastatur']")

# Test 3: Sollte None ergeben
pfad_3 = find_path_in_dict(test_firma, "München")
print(f"Pfad zu 'München':    Dein Ergebnis: {pfad_3} | Richtig wäre: None")```
````

## Rekursion verstehen: Eine Schritt-für-Schritt-Erklärung

Rekursion kann am Anfang verwirrend sein. Man stellt sich eine rekursive Funktion am besten wie eine **Fabrik** vor: Wenn eine Etage nicht weiterweiß, baut sie eine exakte Kopie von sich selbst (eine Tochter-Fabrik) im Keller auf und übergibt ihr die Arbeit.

### 📋 Vorbereitung: Das Dictionary vor Augen

Wir suchen das Wort `"mechanisch"` in diesem verschachtelten Dictionary:

```python
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
```

---

### 🔍 Schritt für Schritt: Der Ablauf

{{< alert icon="circle-info" cardColor="#1e293b" textColor="#f8fafc" >}}

### 🏭 Etage 1: Der allererste Aufruf (Haupt-Dictionary)

Die Funktion startet ganz oben auf der Hauptebene von `test_firma`. Sie geht die Schlüssel (_Keys_) nacheinander durch:

1. **Key: `"name"`**
   - Value ist `"Max"`.
   - Kein Dictionary? Nein.
   - Ist `"Max" == "mechanisch"`? Nein. Die `for`-Schleife springt einfach weiter zum nächsten Key.
2. **Key: `"info"`**
   - Value ist ein Dictionary.
   - **Aktion:** Etage 1 pausiert kurz und öffnet eine neue **Etage 2** im Keller, um in `"info"` zu suchen.
   - _Zeitsprung:_ Da in `"info"` nichts gefunden wird, beendet sich diese Unter-Funktion komplett und gibt `None` zurück. Etage 1 ignoriert das `None` und macht mit ihrer Schleife weiter.
3. **Key: `"gear"`**
   - Value ist wieder ein Dictionary: `{"setup": {...}}`.
   - **Aktion:** Etage 1 pausiert erneut. Sie öffnet eine neue **Etage 2** und übergibt dieses Teil-Dictionary.
     {{< /alert >}}

{{< alert icon="layer-group" >}}

### 🪜 Etage 2: Suche im `"gear"`-Dictionary

Diese Kopie der Funktion weiß nichts mehr von `"name"` oder `"info"`. Sie sieht jetzt nur noch:
`{"setup": {"monitore": "zwei", "tastatur": "mechanisch"}}`

1. **Key: `"setup"`**
   - Value ist wieder ein Dictionary!
   - **Aktion:** Auch Etage 2 pausiert jetzt. Sie öffnet eine **Etage 3** im Keller und übergibt das innere Setup-Dictionary.
     {{< /alert >}}

{{< alert icon="key" >}}

### 🔑 Etage 3: Suche im `"setup"`-Dictionary

Diese Kopie sieht nur noch:
`{"monitore": "zwei", "tastatur": "mechanisch"}`

1. **Key: `"monitore"`**
   - Value ist `"zwei"`. Nicht das, was wir suchen. Weiter in der Schleife.
2. **Key: `"tastatur"`**
   - Value ist `"mechanisch"`. **TREFFER!** 🎉
   - Der `elif`-Block schlägt an. Die Funktion packt den aktuellen Key (`"tastatur"`) in eine Liste: `["tastatur"]`.
   - **Aktion:** Etage 3 schließt sich sofort und ruft nach oben zu Etage 2: _"Ich habe es gefunden! Hier ist das Ergebnis: `["tastatur"]`!"_
     {{< /alert >}}

---

### 🧱 Der Rückweg: Das "Zusammenkleben"

Das ist der Moment im Code, wo das Zusammensetzen (`[key] + result`) passiert. Wir gehen die Treppe mit dem gefundenen Schatz wieder nach oben:

- **Zurück in Etage 2:**
  Hier wartete der Key `"setup"`. Sie empfängt das `result = ["tastatur"]` von unten.
  Sie klebt ihren eigenen Key vorne dran: `["setup"] + ["tastatur"]`.
  Das ergibt: `["setup", "tastatur"]`.
  **Aktion:** Etage 2 schließt sich und reicht das neue Paket weiter hoch.

- **Zurück in Etage 1 (Hauptebene):**
  Hier wartete der Key `"gear"`. Sie empfängt das `result = ["setup", "tastatur"]` von unten.
  Sie klebt ihren eigenen Key vorne dran: `["gear"] + ["setup", "tastatur"]`.
  Das ergibt das finale Ergebnis: **`["gear", "setup", "tastatur"]`**.

Die Hauptebene gibt dieses fertige Paket an dein Hauptprogramm zurück. Die Suche ist erfolgreich beendet!

```python
def find_file_path(nested_folder: dict, target_file: str) -> list or None:
    for key, value in nested_folder.items():

        # 1. Wenn wir die Liste mit Dateien finden
        if type(value) == list:
            if target_file in value:
                # Wir geben direkt die Liste mit dem Dateinamen zurück
                return [target_file]

        # 2. Wenn wir einen Unterordner (Dictionary) finden
        elif type(value) == dict:
            result = find_file_path(value, target_file)

            if result is not None:
                # Wir kleben den Ordnernamen VOR das Ergebnis
                return [key] + result

    # 3. WICHTIG: Erst WENN DIE SCHLEIFE FERTIG IST und nichts fand:
    return None
```

```python
def get_max_depth(nested_dict: dict) -> int:
    # Wenn das Dictionary komplett leer ist ({}), ist die Tiefe 0
    if not nested_dict:
        return 0

    max_sub_depth = 0

    for key, value in nested_dict.items():
        if isinstance(value, dict):
            # 1. Schritt: Starte die Rekursion für das Unter-Dictionary
            # Überlege: Was gibt dir dieser Aufruf zurück?
            sub_depth = get_max_depth(value)

            # 2. Schritt: Wir müssen prüfen, ob dieser Weg tiefer war
            # als alles, was wir in DIESER Schleife bisher gesehen haben.
            # TIPP: Nutze die Python-Funktion max(max_sub_depth, sub_depth)
            # oder ein einfaches if-Statement.
            pass

    # 3. Schritt: Der Rückgabewert für die obere Etage.
    # Wenn max_sub_depth die Tiefe der UNTER-Etagen ist, müssen wir
    # für UNSERE aktuelle Etage noch +1 dazurechnen!
    return max_sub_depth + 1


# --- TESTBEREICH ---
# Tiefe 1: Flach
test_1 = {"a": 1, "b": 2}

# Tiefe 3: "a" -> "b" -> "c"
test_2 = {
    "a": {
        "b": {
            "c": {}
        }
    },
    "d": {
        "e": {} # Dieser Pfad ist nur 2 tief
    }
}

print(f"Tiefe Test 1: Dein Ergebnis: {get_max_depth(test_1)} | Richtig: 1")
print(f"Tiefe Test 2: Dein Ergebnis: {get_max_depth(test_2)} | Richtig: 3")
```
