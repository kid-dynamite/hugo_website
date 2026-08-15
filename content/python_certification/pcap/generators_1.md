+++
date = '2026-08-15T17:22:46+02:00'
draft = false
title = 'Python_generators_1'
showAuthor = false
weight =23
layout = "simple"
summary = "🚀 Python_generators_1"
+++

```python
import sys

x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for element in x:       # stores a list in memory -->
    print(element)

for i in range(1, 11):  # --> more efficient
    print(i)


y = map(lambda i: i**2, x)
print(y)                # <map object at 0x000001E2237AFB50>
#print(list(y))          # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
                        # just created, not storing this list


#for i in y:             # will be calculated while looping
    #print(i)
"""
y = map(...): Python erstellt einen Generator.
Es wird noch kein einziger Wert berechnet.print(list(y)):
Hier zwingst du das map-Objekt, alle seine Werte zu
berechnen und in eine Liste zu packen.
Der Iterator wandert von Anfang bis Ende durch.
for i in y:: Wenn die Schleife startet, steht der Zeiger
des Iterators bereits am Ende. Es sind keine Elemente
mehr übrig, die gedruckt werden könnten. Die Schleife
bricht sofort ab.

Alternative: y = list(map(lambda i: i**2, x))
<-- als echte Liste speichern
"""

print(next(y))       #1
print(next(y))       #4
print(y.__next__())  #9 --> Dunder-Method

"""
for i in y:     # calling next() function
    print(i)

16
25
36
49
64
81
100
"""
while True:         # same as for-loop
    try:
        #value = next(y)
        #print(value)
        print(next(y))
    except StopIteration:
        print("Done")
        break

```
