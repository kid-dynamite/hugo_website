+++
date = '2026-08-13T17:05:02+02:00'
draft = false
title = 'OOP_lab-exercises'
showAuthor = false
weight =21
layout = "simple"
summary = "🚀 OOP lab exercises"
+++

> days of week

```python
class WeekDayError(Exception):
    pass


class Weeker:
    __names = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']

    def __init__(self, day):
        try:
            self.__current = Weeker.__names.index(day)
        except ValueError:
            raise WeekDayError

    def __str__(self):
        return Weeker.__names[self.__current]

    def add_days(self, n):
        self.__current = (self.__current + n) % 7

    def subtract_days(self, n):
        self.__current = (self.__current - n) % 7


try:
    weekday = Weeker('Mon')
    print(weekday)
    weekday.add_days(15)
    print(weekday)
    weekday.subtract_days(23)
    print(weekday)

    print(weekday._Weeker__current)
    #print(weekday.__current)
   # print(_Weeker.__curent_)

    weekday = Weeker('Monday')
except WeekDayError:
    print("Sorry, I can't serve your request.")

"""
Mon
Tue
Sun
6
Sorry, I can't serve your request.
"""
```

> counting stack

```python
class Stack:
    def __init__(self):
        self.__stk = []

    def push(self, val):
        self.__stk.append(val)

    def pop(self):
        val = self.__stk[-1]
        del self.__stk[-1]
        return val

class CountingStack(Stack):
    def __init__(self):
        super().__init__()
        self.__counter = 0

    def get_counter(self):
        return self.__counter

    def pop(self):
        value = Stack.pop(self)
        # Weil die Unterklasse nicht direkt auf _Stack__stk zugreifen kann,
        # musst du Methoden der Elternklasse aufrufen (Stack.pop(self) oder
        # super().pop()), um an die Daten heranzukommen.
        self.__counter += 1
        return value

stk = CountingStack()
for i in range(100):
    stk.push(i)
    stk.pop()
print(stk.get_counter())


print(stk.__dict__)
print(stk._CountingStack__counter)


"""
stk._Stack__stk = "hello"
print(stk._Stack__stk)
"""
```

> points_on_a_plane

```python
import math

class Point:
    def __init__(self, x=0.0, y=0.0):
        # Koordinaten als private Variablen (PCAP-Konvention) oder Standard-Attribute
        self.__x = float(x)
        self.__y = float(y)

    def getx(self):
        return self.__x

    def gety(self):
        return self.__y

    def distance_from_point(self, point):
        # Berechnet Abstand zu einem anderen Point-Objekt
        return math.hypot(self.__x - point.getx(), self.__y - point.gety())

    def distance_from_xy(self, x, y):
        # Berechnet Abstand zu rohen X/Y-Koordinaten
        return math.hypot(self.__x - x, self.__y - y)


# --- Testcode entsprechend den Kursvorgaben ---
point1 = Point(0, 0)
point2 = Point(3, 4)

print(point1.distance_from_point(point2))  # Erwartete Ausgabe: 5.0
print(point2.distance_from_xy(2, 0))       # Erwartete Ausgabe: 4.123105625617661
```

> timer class

```python

```
