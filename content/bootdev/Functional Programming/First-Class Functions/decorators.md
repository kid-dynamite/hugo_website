+++
date = '2026-08-28T17:27:00+02:00'
draft = false
title = 'Decorators'
showAuthor = false
weight =54
layout = "simple"
summary = "🚀 decorators"
+++

```python
# 1. Der Decorator nimmt die originale Funktion als Variable auf
def mein_decorator(original_funktion):

    # 2. Die Verpackung
    def wrapper():
        print("[VORHER] Code vor der Funktion")

        original_funktion()  # Hier wird die echte Funktion ausgeführt

        print("[NACHHER] Code nach der Funktion")

    # 3. Wir geben die fertige Verpackung zurück
    return wrapper



@mein_decorator
def code_ausfuehren():
    print("-> Ich lerne Decorators! <-")

# Wenn du das jetzt aufrufst:
code_ausfuehren()


"""

[VORHER] Code vor der Funktion
-> Ich lerne Decorators! <-
[NACHHER] Code nach der Funktion

code_ausfuehren = mein_decorator(code_ausfuehren)

"""

```

```python
def mein_decorator(original_funktion):
    # Die Joker (*args, **kwargs) fangen JEDE Variable ab (z.B. den Namen "Anna")
    def wrapper(*args, **kwargs):
        print("[VORHER] Code vor der Funktion")

        # Hier leiten wir die Variablen an die echte Funktion weiter!
        original_funktion(*args, **kwargs)

        print("[NACHHER] Code nach der Funktion")

    return wrapper

@mein_decorator
def begruesse_user(name):
    print(f"-> Hallo {name}! <-")

# Aufruf
begruesse_user("Anna")

"""
[VORHER] Code vor der Funktion
-> Hallo Anna! <-
[NACHHER] Code nach der Funktion
"""
```

```python
def perfekter_decorator(original_funktion):
    def wrapper(*args, **kwargs):
        print("[VORHER]")

        # 1. Berechne das Ergebnis und speichere es in einer Variable!
        echtes_ergebnis = original_funktion(*args, **kwargs)

        print("[NACHHER]")

        # 2. WICHTIG: Gib das Ergebnis an den Code zurück!
        return echtes_ergebnis

    return wrapper

@perfekter_decorator
def addiere(a, b):
    return a + b

ergebnis = addiere(5, 10)
print(ergebnis)


```

> Boot.dev exercise

```python
from collections.abc import Callable


def vowel_counter(func_to_decorate: Callable[[str], None]) -> Callable[[str], None]:
    vowel_count: int = 0

    def wrapper(doc: str) -> None:
        nonlocal vowel_count
        vowels: str = "aeiou"
        for char in doc:
            if char.lower() in vowels:
                vowel_count += 1
        print(f"Vowel count: {vowel_count}")
        func_to_decorate(doc)

    return wrapper


@vowel_counter
def process_doc(doc: str) -> None:
    print(f"Document: {doc}")


process_doc("What")
# Vowel count: 1
# Document: What

process_doc("A wonderful")
# Vowel count: 5
# Document: A wonderful

process_doc("world")
# Vowel count: 6
# Document: world

"""
# Erstellung ohne decorator
def process_doc(doc: str) -> None:
    print(f"Document: {doc}")


process_doc = vowel_counter(process_doc)
process_doc("Something wicked this way comes")
"""
```
