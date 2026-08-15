+++
date = '2026-08-14T19:04:03+02:00'
draft = false
title = 'OOP_inheritance'
showAuthor = false
weight =23
layout = "simple"
summary = "🚀 OOP inheritance"
+++

```python
class Level1:
    variable_1 = 100
    def __init__(self):
        self.var_1 = 101

    def fun_1(self):
        return 102


class Level2(Level1):
    variable_2 = 200
    def __init__(self):
        super().__init__()
        self.var_2 = 201

    def fun_2(self):
        return 202

class Level3(Level2):
    variable_3 = 300
    def __init__(self):
        super().__init__()
        self.var_3 = 301

    def fun_3(self):
        return 302

obj = Level3()

print(obj.variable_1, obj.var_1, obj.fun_1())
print(obj.variable_2, obj.var_2, obj.fun_2())
print(obj.variable_3, obj.var_3, obj.fun_3())

"""
100 101 102
200 201 202
300 301 302
"""
```

```python
class Device:
    def __init__(self, status):
        self.status = status
    def action(self):
        return not self.status

class Screen(Device):
    def action(self):
        return self.status

class System:
    def __init__(self, hardware):
        self.hardware = hardware
    def run(self):
        return self.hardware.action()

dev1 = Screen(False)
dev2 = Device(False)
sys1 = System(dev1)
sys2 = System(dev2)

print(int(sys1.run()), int(sys2.run()))

"""
B) 0 1
"""
```

## 🔍 Der Lösungsweg Schritt für Schritt

Hier ist die genaue Analyse, wie die Prüfung diesen Code auswertet:

### 1. Die Instanziierung (Objekterzeugung)

- **`dev1 = Screen(False)`**  
  `Screen` erbt von `Device`. Da `Screen` keinen eigenen Konstruktor (`__init__`) hat, wird der Konstruktor von `Device` aufgerufen. `dev1.status` wird auf `False` gesetzt.
- **`dev2 = Device(False)`**  
  `dev2.status` wird ebenfalls auf `False` gesetzt.

### 2. Die Kopplung (Dependency Injection)

- `sys1 = System(dev1)` — `sys1.hardware` hält das `Screen`-Objekt.
- `sys2 = System(dev2)` — `sys2.hardware` hält das `Device`-Objekt.

### 3. Die Auswertung von `sys1.run()`

`sys1.run()` ruft `self.hardware.action()` auf. Da `hardware` hier das `Screen`-Objekt (`dev1`) ist, wird die Methode `action()` in der Klasse `Screen` ausgeführt.

- **Code-Logik:** `return self.status`
- **Ergebnis:** Da `dev1.status` den Wert `False` hat, gibt `sys1.run()` den Wert **`False`** zurück.

### 4. Die Auswertung von `sys2.run()`

`sys2.run()` ruft ebenfalls `self.hardware.action()` auf. Da `hardware` hier das `Device`-Objekt (`dev2`) ist, wird die Methode `action()` in der Basisklasse `Device` ausgeführt.

- **Code-Logik:** `return not self.status`
- **Ergebnis:** Da `dev2.status` den Wert `False` hat, kehrt `not False` um zu **`True`**.

### 5. Die Ausgabe (`print`)

- `int(False)` wandelt sich in Python zu `0`.
- `int(True)` wandelt sich in Python zu `1`.

Das Ergebnis in der Konsole lautet somit: **`0 1`**

> Population

```python
class Mouse:
    Population = 0
    def __init__(self, name):
        Mouse.Population += 1  # Verändert die globale Klassenvariable
        self.name = name

# Zwei Mäuse werden erstellt
m1 = Mouse("Mickey")
m2 = Mouse("Minnie")

print(Mouse.Population)  # Ausgabe: 2 (Korrekte Gesamtanzahl)
print(m1.Population)     # Ausgabe: 2 (Greift auf die Klassenvariable zu)
print(m2.Population)     # Ausgabe: 2 (Greift auf die Klassenvariable zu)

#************************************************************

class Mouse:
    Population = 0
    def __init__(self, name):
        self.Population += 1  # Erstellt eine eigene Instanzvariable
        self.name = name

# Zwei Mäuse werden erstellt
m1 = Mouse("Mickey")
m2 = Mouse("Minnie")

print(Mouse.Population)  # Ausgabe: 0 (Die Klassenvariable wurde nie erhöht!)
print(m1.Population)     # Ausgabe: 1 (Nur m1 hat den Wert 1)
print(m2.Population)     # Ausgabe: 1 (Auch m2 hat den Wert 1)

```
