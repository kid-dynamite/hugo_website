+++
date = '2026-08-23T22:16:52+02:00'
draft = false
title = 'Closures'
showAuthor = false
weight =51
layout = "simple"
summary = "🚀 closures"
+++

```python
def erstelle_zaehler():
    anzahl = 0  # Diese Variable "merkt" sich die Closure

    def zaehler():
        nonlocal anzahl  # Erlaubt es, die äußere Variable zu ändern
        anzahl += 1
        return anzahl

    return zaehler  # Wir geben die Funktion selbst zurück, ohne sie aufzurufen

# Anwendung:
mein_zaehler = erstelle_zaehler()

print(mein_zaehler())  # Ausgabe: 1
print(mein_zaehler())  # Ausgabe: 2

```

```python
def multipliziere_mit(faktor):
    def funktion(zahl):
        return zahl * faktor
    return funktion

verdoppler = multipliziere_mit(2)
verdreifacher = multipliziere_mit(3)

print(verdoppler(5))     # Ausgabe: 10 (5 * 2)
print(verdreifacher(5))  # Ausgabe: 15 (5 * 3)
```

```python
def erstelle_spieler(name, start_hp):
    # Diese Variablen sind von außen geschützt (Kapselung)
    spieler_name = name
    hp = start_hp

    # Die innere Funktion (die Closure)
    def schaden_zufuegen(menge):
        nonlocal hp  # Erlaubt das Ändern der Variable im äußeren Scope
        hp -= menge
        if hp <= 0:
            return f"{spieler_name} wurde besiegt!"
        return f"{spieler_name} hat noch {hp} HP übrig."

    # Wir geben die innere Funktion zurück
    return schaden_zufuegen

# --- Anwendung ---

# Wir erstellen einen konkreten Spieler
held_angreifen = erstelle_spieler("Arthur", 100)

# Jetzt fügen wir Schaden zu
print(held_angreifen(30))  # Ausgabe: Arthur hat noch 70 HP übrig.
print(held_angreifen(50))  # Ausgabe: Arthur hat noch 20 HP übrig.
print(held_angreifen(25))  # Ausgabe: Arthur wurde besiegt!
```

### 1. Die Typen-Signatur lesen

Schau dir den Rückgabetyp genau an:

```python
-> Callable[[str, str, str], Styles]
```

Das bedeutet übersetzt:

- **Äußere Funktion:** Deine Funktion `css_styles` muss eine _innere Funktion_ zurückgeben.
- **Innere Funktion:** Diese innere Funktion muss genau **drei Argumente** entgegennehmen (alle vom Typ `str`).
- **Rückgabewert:** Am Ende muss diese innere Funktion ein Dictionary vom Typ `Styles` zurückgeben.

---

### 2. Das `copy`-Modul nutzen

Oben im Code steht nicht ohne Grund `import copy`.

{{< alert type="warning" card="true" >}}
**Achtung bei Mutationen!**  
In der funktionalen Programmierung wollen wir Seiteneffekte und Mutationen vermeiden (_Pure Functions_). Wenn du das originale `initial_styles` direkt veränderst, manipulierst du den Zustand außerhalb der Closure.
{{< /alert >}}

Nutze in deiner inneren Funktion `copy.deepcopy()`, um eine echte Kopie von `initial_styles` (oder dem aktuellen Zustand) zu erstellen, bevor du Einträge hinzufügst oder änderst.

---

### 3. Brauchst du `nonlocal`?

Das hängt ganz von deinem gewählten Ansatz ab:

- **Variante A:** Wenn du den Zustand von `initial_styles` bei jedem Aufruf dauerhaft in der Closure verändern und speichern willst, hilft dir eine Kopie und eventuell das Keyword `nonlocal`.
- **Variante B:** Wenn die Funktion einfach nur auf Basis der `initial_styles` eine neue, erweiterte Kopie zurückgeben soll, reicht der normale Lesezugriff auf die äußere Variable völlig aus.

---

### 🚀 Dein Fahrplan im Code

Folge diesen Schritten, um die Aufgabe Schritt für Schritt zu lösen:

1. **Innere Funktion definieren:** Erstelle innerhalb von `css_styles` eine neue Funktion mit 3 Parametern.
2. **Kopieren:** Kopiere die Styles innerhalb dieser inneren Funktion mit `copy.deepcopy()`.
3. **Modifizieren:** Bearbeite die Kopie anhand der 3 Parameter (wahrscheinlich ein CSS-Selector, eine Property und ein Value).
4. **Zurückgeben (Innen):** Gib die modifizierte Kopie am Ende der inneren Funktion zurück.
5. **Zurückgeben (Außen):** Gib ganz unten in der äußeren Funktion die innere Funktion selbst zurück (`return name_der_inneren_funktion`).
