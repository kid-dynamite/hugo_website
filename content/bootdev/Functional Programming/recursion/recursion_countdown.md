+++
date = '2026-08-04T10:07:15+02:00'
draft = false
title = 'Recursion_countdown'
showAuthor = false
weight =58
layout = "simple"
summary = "🚀 simple countdown exercises"
+++

> Exercise

```python
def countdown(num):
    if num == 0:
        return 1

    print(f"bevor (Hinweg): {num}")

    a = countdown(num-1)

    # JETZT DRUCKEN WIR A:
    print(f"   <- Rückweg bei num={num}: Das 'a' hat gerade den Wert {a} erhalten!")

    neuer_wert = a + 5
    return neuer_wert

# Wir fangen das Endergebnis in einer Variable auf und drucken es
endergebnis = countdown(5)
print(f"\nDas finale Endergebnis ist: {endergebnis}")
```

> prints...

```python
"""
Ausgabe:
bevor (Hinweg): 5
bevor (Hinweg): 4
bevor (Hinweg): 3
bevor (Hinweg): 2
bevor (Hinweg): 1
   <- Rückweg bei num=1: Das 'a' hat gerade den Wert 1 erhalten!
   <- Rückweg bei num=2: Das 'a' hat gerade den Wert 6 erhalten!
   <- Rückweg bei num=3: Das 'a' hat gerade den Wert 11 erhalten!
   <- Rückweg bei num=4: Das 'a' hat gerade den Wert 16 erhalten!
   <- Rückweg bei num=5: Das 'a' hat gerade den Wert 21 erhalten!

Das finale Endergebnis ist: 26
"""
```

> Exercise

```python
def countdown_summe(num):
    # Basiszahl (Abbruchbedingung)
    if num == 0:
        return 0  # Wenn wir bei 0 sind, starten wir die Summe mit 0

    print(f"Auf dem Hinweg (Zahl wird gemerkt): {num}")

    # REKURSIONS-SCHRITT mit return:
    # Wir sagen: "Das Ergebnis ist aktuelle Zahl + das Ergebnis vom Rest"
    ergebnis = num + countdown_summe(num - 1)

    print(f"Auf dem Rückweg (Rechnung): {num} + Ergebnis vom Rest = {ergebnis}")
    return ergebnis

# Testaufruf
gesamt_summe = countdown_summe(3)
print(f"Endergebnis: {gesamt_summe}")
```

> prints

```python
"""
Ausgabe:
Auf dem Hinweg (Zahl wird gemerkt): 3
Auf dem Hinweg (Zahl wird gemerkt): 2
Auf dem Hinweg (Zahl wird gemerkt): 1
Auf dem Rückweg (Rechnung): 1 + Ergebnis vom Rest = 1
Auf dem Rückweg (Rechnung): 2 + Ergebnis vom Rest = 3
Auf dem Rückweg (Rechnung): 3 + Ergebnis vom Rest = 6
Endergebnis: 6
"""
```

> Exercise

```python
def countdown_string(num):
    if num == 0:
        return "Start!" # Das Fundament am Ende der Kette

    # REKURSIONS-SCHRITT:
    # Wir holen uns den Text der nächsten Ebene und hängen unsere Zahl vorne an
    rueckgabe_von_unten = countdown_string(num - 1)

    mein_text = f"{num} -> {rueckgabe_von_unten}"
    return mein_text

# Testaufruf
print(countdown_string(5))
# Ausgabe: 5 -> 4 -> 3 -> 2 -> 1 -> Start!
```

```python
def countdown_string(num):
    if num == 0:
        return "Start!" # Das Fundament am Ende der Kette

    # REKURSIONS-SCHRITT:
    # Wir holen uns den Text der nächsten Ebene und hängen unsere Zahl vorne an
    rueckgabe_von_unten = countdown_string(num - 1)

    mein_text = f"{num} -> {rueckgabe_von_unten}"
    print(mein_text)
    return mein_text

# Testaufruf
print(countdown_string(5))


"""
1 -> Start!
2 -> 1 -> Start!
3 -> 2 -> 1 -> Start!
4 -> 3 -> 2 -> 1 -> Start!
5 -> 4 -> 3 -> 2 -> 1 -> Start!
5 -> 4 -> 3 -> 2 -> 1 -> Start!
"""

```
