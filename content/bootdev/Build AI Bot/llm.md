+++
date = '2026-09-02T23:42:34+02:00'
draft = false
title = 'LLMs'
showAuthor = false
weight =10
layout = "simple"
summary = "🚀 LLMs"
+++

```python
class Zimmer:
    def __init__(self, groesse_qm):
        self.groesse_qm = groesse_qm  # Das entspricht "prompt_tokens"

class Haus:
    def __init__(self):
        # Das entspricht "usage". Am Anfang ist das Haus leer (None).
        self.wohnzimmer = None

# --- Anwendung in der Praxis ---

mein_haus = Haus()  # Wir erstellen ein Haus-Objekt

# 1. Die Prüfung (Wie in deiner Boot.dev Aufgabe)
if mein_haus.wohnzimmer is None:
    print("Fehler: Das Haus hat noch gar kein Wohnzimmer aufgebaut!")

# 2. Jetzt bauen wir das Zimmer und fügen es dem Haus hinzu (Objekt-Komposition)
neues_zimmer = Zimmer(40)
mein_haus.wohnzimmer = neues_zimmer  # Das Attribut zeigt jetzt auf ein Objekt!

# 3. Jetzt ist die Prüfung False
if mein_haus.wohnzimmer is None:
    print("Wird nicht ausgeführt.")
else:
    # Per Punkt-Notation durchreichen: haus -> wohnzimmer -> groesse_qm
    print(f"Das Wohnzimmer ist aufgebaut und hat {mein_haus.wohnzimmer.groesse_qm} qm.")


# mein_haus     responseDas     Hauptobjekt (Der Container)
# .wohnzimmer   .usage          Eine Variable im Hauptobjekt, die auf ein anderes Objekt zeigt (oder auf None)
# .groesse_qm   .prompt_tokens  Die eigentliche Variable (Zahl/String) im inneren Objekt

# Dass eine Variable innerhalb einer Klasse (self.wohnzimmer) auf eine Instanz einer völlig anderen Klasse
# (Zimmer) verweisen kann, ist der nächste Schritt in der Software-Architektur (genannt Objekt-Komposition).

```

```python
class Balkon:
    def __init__(self, groesse):
        self.groesse = groesse

class Zimmer:
    def __init__(self, groesse_qm):
        self.groesse_qm = groesse_qm
        self.balkon = Balkon(5)  # Das Zimmer hat jetzt ein Balkon-Objekt

class Haus:
    def __init__(self):
        self.wohnzimmer = Zimmer(40)

# Der Zugriff funktioniert jetzt genau über diese lange Kette:
print(mein_haus.wohnzimmer.balkon.groesse)  # Ausgabe: 5
```
