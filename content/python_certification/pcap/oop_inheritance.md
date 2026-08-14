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
