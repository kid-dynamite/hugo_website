+++
date = '2026-08-28T17:27:00+02:00'
draft = false
title = 'Decorators'
showAuthor = false
weight =54
layout = "simple"
summary = "🚀 decorators"
+++

> \*args & \*\*kwargs

```python
def log_call(func):
    def wrapper(*args, **kwargs):
        # 1. Funktion ausführen
        result = func(*args, **kwargs)

        # 2. Argumente sauber als Strings sammeln
        arg_strings = []
        for arg in args:
            arg_strings.append(str(arg))
        for key, value in kwargs.items():
            arg_strings.append(f"{key}={str(value)}")

        # 3. String zusammenbauen
        arg_list_str = ", ".join(arg_strings)
        return f"{func.__name__}({arg_list_str}) -> {str(result)}"

    return wrapper

# --- Dekorierte Funktionen ---

@log_call
def add(a, b):
    return a + b

@log_call
def format_name(first, last, title=None):
    if title is None:
        return f"{first} {last}"
    return f"{title} {first} {last}"

@log_call
def greet(name, excited=False):
    if excited:
        return f"Hello, {name}!"
    return f"Hello, {name}"

# --- Hardcoded Test-Ausgaben ---

print(add(2, 3))
print(format_name("Ada", "Lovelace", title="Countess"))
print(greet("Mario"))
print(greet("Link", excited=True))
```

```python
from collections.abc import Callable
from functools import wraps

# Eine Funktion, die nichts entgegennimmt, aber einen float zurückgibt
PreisFunktion = Callable[[], float]

# 1. TODO: Dekorator mit Argument für den Rabatt (z.B. 10 für 10%)
def mit_rabatt(prozent: float):
    def dekorator(func: PreisFunktion) -> PreisFunktion:
        @wraps(func)
        def wrapper() -> float:
            # 1. Rufe func() auf, um den aktuellen Preis zu bekommen
            # 2. Berechne den Rabatt
            # 3. Gib den neuen Preis mit return zurück
            pass
        return wrapper
    return dekorator

# 2. TODO: Dekorator ohne Argument für 19% Mehrwertsteuer
def mit_mwst(func: PreisFunktion) -> PreisFunktion:
    @wraps(func)
    def wrapper() -> float:
        # 1. Rufe func() auf
        # 2. Rechne * 1.19
        # 3. Gib den neuen Preis mit return zurück
        pass
    return wrapper

# 3. TODO: Wende die Dekoratoren so an, dass ZUERST Rabatt und DANN MwSt berechnet wird
def berechne_grundpreis() -> float:
    return 100.0

# Testaufruf
endpreis = berechne_grundpreis()
print(f"Der finale Endpreis ist: {endpreis:.2f} €")
# Ziel-Ergebnis bei 10% Rabatt: 107.10 €
```

```python
from collections.abc import Callable
from functools import wraps

KaffeeFunktion = Callable[[], None]


# Dekorator mit Argumenten: Akzeptiert die Anzahl der Zuckerstücke
def mit_zucker(anzahl: int = 2):
    # Dies ist der eigentliche Dekorator
    def dekorator(func: KaffeeFunktion) -> KaffeeFunktion:
        @wraps(func)
        def wrapper():
            print(f"🍬 {anzahl} Stück Zucker einrühren...")
            func()

        return wrapper

    return dekorator


# Dekorator ohne Argumente (wie vorher)
def mit_milchschaum(func: KaffeeFunktion) -> KaffeeFunktion:
    @wraps(func)
    def wrapper():
        print("🥛 Milchschaum aufschäumen und hinzufügen...")
        func()

    return wrapper


# Anwendung mit individuellem Argument
@mit_milchschaum
@mit_zucker(anzahl=3)  # Hier übergeben wir die 3
def mache_kaffee():
    print("☕ Schwarzer Kaffee läuft in die Tasse.")


mache_kaffee()
"""
🥛 Milchschaum aufschäumen und hinzufügen...
🍬 3 Stück Zucker einrühren...
☕ Schwarzer Kaffee läuft in die Tasse.
"""
```

```python
def replacer(old, new):
    def replace(decorated_func):
        def wrapper(text):
            # Ersetzt das alte Zeichen durch das neue und gibt es weiter
            return decorated_func(text.replace(old, new))

        return wrapper

    return replace


# Die Dekoratoren werden von oben nach unten auf den Text angewendet
@replacer("&", "&amp;")
@replacer("<", "&lt;")
@replacer(">", "&gt;")
@replacer('"', "&quot;")
@replacer("'", "&#x27;")
def tag_pre(text):
    # Diese Funktion wird ganz zum Schluss ausgeführt
    return f"<pre>{text}</pre>"


# --- TESTAUFRUFE ---

# Test 1: Ein HTML-Link als Text
eingabe_1 = '<a href="https://boot.dev">Link & Info</a>'
ausgabe_1 = tag_pre(eingabe_1)

print("TEST 1:")
print(f"Eingabe: {eingabe_1}")
print(f"Ausgabe: {ausgabe_1}")
print("-" * 40)

# Test 2: Text mit Anführungszeichen
eingabe_2 = "Das ist 'wichtig' & <cool>"
ausgabe_2 = tag_pre(eingabe_2)

print("TEST 2:")
print(f"Eingabe: {eingabe_2}")
print(f"Ausgabe: {ausgabe_2}")
```

```python
from collections.abc import Callable

TextFunc = Callable[[str], None]

def set_par(func):
    def wrapper(document):
        par = f"({document})"
        func(par)
    return wrapper


def to_uppercase(func: TextFunc) -> TextFunc:
    def wrapper(document: str) -> None:
        func(document.upper())

    return wrapper


def get_truncate(length: int) -> Callable[[TextFunc], TextFunc]:
    def truncate(func: TextFunc) -> TextFunc:
        def wrapper(document: str) -> None:
            func(document[:length])

        return wrapper

    return truncate

@set_par
@to_uppercase
@get_truncate(9)  # currying
def print_input(input: str) -> None:
    print(input)


print_input("Keep Calm and Carry On")

# prints: (KEEP CAL
```

```python

def get_replace(func):
def wrapper(*args):
return func(*args)
return wrapper

def to_uppercase(func):
def wrapper(*args):
result = func(*args)
return result.upper()
return wrapper

@to_uppercase
@get_replace
def replace(old_str: str, old: str, new: str):
new_str = "".join(map(lambda x: new if x == old else x, old_str))
return new_str

print(replace("<pre>&lt;p&gt;This paragraph has &lt;em&gt;italic text&lt;/em&gt;&lt;/p&gt;</pre>", "&", "&amp;"))

#do_replace = get_replace(replace)
#print(do_replace("<pre>&lt;p&gt;This paragraph has &lt;em&gt;italic text&lt;/em&gt;&lt;/p&gt;</pre>", "&", "&amp;"))

# print(replace("<pre>&lt;p&gt;This paragraph has &lt;em&gt;italic text&lt;/em&gt;&lt;/p&gt;</pre>", "&", "&amp;"))

def get_replace(func):
def wrapper(*args): # Reicht die Argumente einfach unverändert nach unten weiter
return func(*args)
return wrapper

def to_uppercase(func):
def wrapper(old_str: str, old: str, new: str): # 1. Wir machen den Text GROSS, BEVOR wir die nächste Funktion aufrufen
large_str = old_str.upper() # 2. Wir rufen func auf, übergeben aber den bereits veränderten Text
return func(large_str, old, new)
return wrapper

@to_uppercase
@get_replace
def replace(old_str: str, old: str, new: str): # Diese Funktion bekommt jetzt schon den fertigen GROSSEN Text geliefert!
new_str = "".join(map(lambda x: new if x == old else x, old_str))
return new_str

# Testaufruf

print(replace("<pre>&lt;p&gt;This paragraph has &lt;em&gt;italic text&lt;/em&gt;&lt;/p&gt;</pre>", "&", "&amp;"))

```

```python
def vorher_nachher(alte_funktion):
    def neue_funktion(*args, **kwargs):
        print("das kommt vor der Funktion")
        alte_funktion(*args, **kwargs)
        print("das kommt nach der Funktion")
    return neue_funktion

# Ein neuer, zweiter Decorator
def sternchen_rahmen(alte_funktion):
    def neue_funktion(*args, **kwargs):
        print("***************************************")
        alte_funktion(*args, **kwargs)
        print("***************************************")
    return neue_funktion


# Der oberste Decorator umschließt alles, was darunter passiert
@sternchen_rahmen
@vorher_nachher
def hello_world(x, y=2):
    print("hello_world_" * x * y)


# Hinter den Kulissen passiert jetzt genau das:
# hello_world = sternchen_rahmen(vorher_nachher(hello_world))


hello_world(1)

print("+++++++++++++++++++++++++++++++++++++++")

hello_world(2)
```

```python
from collections.abc import Callable

# Ein Typ-Hinweis: Eine Funktion, die nichts zurückgibt (None)
KaffeeFunktion = Callable[[], None]


# Dekorator 1: Fügt Milchschaum hinzu
def mit_milchschaum(func: KaffeeFunktion) -> KaffeeFunktion:
    def wrapper():
        print("🥛 Milchschaum aufschäumen und hinzufügen...")
        func()  # Hier wird der eigentliche Kaffee gekocht

    return wrapper


# Dekorator 2: Fügt Zucker hinzu
def mit_zucker(func: KaffeeFunktion) -> KaffeeFunktion:
    def wrapper():
        print("🍬 2 Stück Zucker einrühren...")
        func()  # Hier wird der eigentliche Kaffee gekocht

    return wrapper


# Wir wenden beide Dekoratoren an
@mit_milchschaum
@mit_zucker
def mache_kaffee():
    print("☕ Schwarzer Kaffee läuft in die Tasse.")


# Jetzt rufen wir die Funktion auf
mache_kaffee()


"""
🥛 Milchschaum aufschäumen und hinzufügen...
🍬 2 Stück Zucker einrühren...
☕ Schwarzer Kaffee läuft in die Tasse.

"""
```

```python
from collections.abc import Callable

# 1. Typ-Definition (Muss ganz oben stehen)
LogFunc = Callable[[str], None]


# 2. Dekorator 1: Wiederholung (3 Ebenen)
def wiederhole(anzahl: int) -> Callable[[LogFunc], LogFunc]:
    def eigentlicher_dekorator(func: LogFunc) -> LogFunc:
        def wrapper(text: str) -> None:
            for _ in range(anzahl):
                func(text)

        return wrapper

    return eigentlicher_dekorator


# 3. Dekorator 2: Log-Präfix (2 Ebenen)
def fuege_log_praefix_hinzu(func: LogFunc) -> LogFunc:
    def wrapper(text: str) -> None:
        neuer_text = f"[LOG]: {text}"
        func(neuer_text)

    return wrapper


# 4. Anwendung BEIDER Dekoratoren auf die Funktion
@fuege_log_praefix_hinzu  # Wird als ZWEITES angewendet
@wiederhole(3)  # Wird als ERSTES angewendet
def zeige_nachricht(text: str) -> None:
    print(text)


# 5. Ausführen
zeige_nachricht("Hilfe!")
"""
[LOG]: Hilfe!
[LOG]: Hilfe!
[LOG]: Hilfe!
"""
```

```python
from collections.abc import Callable

# Ein Typ-Alias für Funktionen, die einen String nehmen und nichts zurückgeben
LogFunc = Callable[[str], None]


def fuege_log_praefix_hinzu(func: LogFunc) -> LogFunc:
    def wrapper(text: str) -> None:
        # Hier verändern wir das Verhalten: Wir modifizieren den Text vor dem Aufruf
        neuer_text = f"[LOG]: {text}"
        func(neuer_text)  # Ruft die originale Funktion auf

    return wrapper  # Wir geben die Verpackung zurück
```

```python
def show_log(alte_funktion):
    def neue_funktion(*args):
        # 1. Wir müssen den String und das Ergebnis verbinden (oder getrennt nutzen)
        # 2. Wir müssen die alte Funktion richtig aufrufen
        print("[LOG]:", end=" ")  # Gibt das Präfix ohne Zeilenumbruch aus
        alte_funktion(*args)      # Ruft die originale Funktion auf

    return neue_funktion


@show_log  # Hier aktivieren wir deinen Dekorator!
def zeige_nachricht(text: str) -> None:
    print(text)


zeige_nachricht("Hallo Welt")
# Ausgabe: [LOG]: Hallo Welt
```

> Basics decorators

```python
def vorher_nachher(alte_funktion):
    def neue_funktion(*args, **kwargs):
        print("das kommt vor der Funktion")
        alte_funktion(*args, **kwargs)
        print("das kommt nach der Funktion")
    return neue_funktion


@vorher_nachher
def hello_world(x, y=2):
    print("hello_world_" * x * y)


# hello_world = vorher_nachher(hello_world)


hello_world(1)

print("+++++++++++++++++++++++++++++++++++++++")


hello_world(2)

"""
das kommt vor der Funktion
hello_world_hello_world_
das kommt nach der Funktion
+++++++++++++++++++++++++++++++++++++++
das kommt vor der Funktion
hello_world_hello_world_hello_world_hello_world_
das kommt nach der Funktion
"""
```

> Basics \*args & \*\*kwargs

```python
def display_name(*args):
    for arg in args:
        print(arg, end=" ")


display_name("alex", "python", "Spongebob")

def print_address(**kwargs):
    for key, value in kwargs.items():
        print(key)
        print(value)


print_address(street="123 Fake St.",
              city="Detroit",
              state="MI",
              zip=1234)

def func(a, b, c):
    print(a, b, c)


args = (1, 2)
kwargs = {'c': 3}

func(*args, **kwargs)
```

```python
from collections.abc import Callable

# 1. Die Funktion, die das Markdown bereinigt
def convert_md_to_txt(doc: str) -> str:
    lines = doc.split("\n")
    for i in range(len(lines)):
        line = lines[i]
        lines[i] = line.lstrip("# ")
    return "\n".join(lines)


# 2. Der Decorator
def markdown_to_text_decorator(func: Callable[..., str]) -> Callable[..., str]:
    def wrapper(*args: str, **kwargs: str) -> str:
        converted_args: list[str] = list(map(convert_md_to_txt, args))

        # converted_args = [convert_md_to_txt(arg) for arg in args]   # list comprehension
        # converted_kwargs = {k: convert_md_to_txt(v) for k, v in kwargs.items()}

        def kwarg_item_to_txt(item_tuple: tuple[str, str]) -> tuple[str, str]:
            key, value = item_tuple
            return (key, convert_md_to_txt(value))

        converted_kwargs: dict[str, str] = dict(map(kwarg_item_to_txt, kwargs.items()))
        return func(*converted_args, **converted_kwargs)
    return wrapper


# 3. Die dekorierten Funktionen
@markdown_to_text_decorator
def concat(first_doc: str, second_doc: str) -> str:
    return f"  First: {first_doc}\n  Second: {second_doc}"


@markdown_to_text_decorator
def format_as_essay(title: str, body: str, conclusion: str) -> str:
    return f"  Title: {title}\n  Body: {body}\n  Conclusion: {conclusion}"


# 4. Direkte Ausführung mit festen Werten
if __name__ == "__main__":
    print("=== RUNNING DECORATOR TESTS ===")

    # Test 1: Positionelle Argumente (args)
    print("\n[Test 1] Aufruf von concat() mit Positionellen Argumenten:")
    raw_arg1 = "# We like to play it all"
    raw_arg2 = "## Welcome to Tally Hall"

    print(f"  Input 1: '{raw_arg1}'")
    print(f"  Input 2: '{raw_arg2}'")
    print("  --- Output ---")

    output1 = concat(raw_arg1, raw_arg2)
    print(output1)
    print("-" * 50)

    # Test 2: Keyword Argumente (kwargs)
    print("\n[Test 2] Aufruf von format_as_essay() mit Keyword Argumenten:")
    raw_title = "# Why Python is Great"
    raw_body = "Maybe it isn't"
    raw_conclusion = "## That's why Python is great!"

    print(f"  Input title:      '{raw_title}'")
    print(f"  Input body:       '{raw_body}'")
    print(f"  Input conclusion: '{raw_conclusion}'")
    print("  --- Output ---")

    output2 = format_as_essay(title=raw_title, body=raw_body, conclusion=raw_conclusion)
    print(output2)
    print("=" * 31)

"""
=== RUNNING DECORATOR TESTS ===

[Test 1] Aufruf von concat() mit Positionellen Argumenten:
  Input 1: '# We like to play it all'
  Input 2: '## Welcome to Tally Hall'
  --- Output ---
  First: We like to play it all
  Second: Welcome to Tally Hall
--------------------------------------------------

[Test 2] Aufruf von format_as_essay() mit Keyword Argumenten:
  Input title:      '# Why Python is Great'
  Input body:       'Maybe it isn't'
  Input conclusion: '## That's why Python is great!'
  --- Output ---
  Title: Why Python is Great
  Body: Maybe it isn't
  Conclusion: That's why Python is great!
===============================

"""
```

### 1. Das Einzel-Sternchen `*args` (Packt in ein Tupel)

- **Was es tut:** Es sammelt alle **namenlosen** Argumente (Positional Arguments) ein und steckt sie in ein **Tupel**.
- **Wann nutzt man es:** Wenn du vorher nicht weißt, wie viele Werte übergeben werden.
  - _Beispiel:_ Eine Funktion, die Zahlen addiert: `addiere(1, 2)` oder `addiere(1, 2, 3, 4, 5)`.

### 2. Das Doppel-Sternchen `**kwargs` (Packt in ein Dictionary)

- **Was es tut:** Es sammelt alle **benannten** Argumente (Keyword Arguments) ein und steckt sie in ein **Dictionary**.
- **Wann nutzt man es:** Wenn du Optionen, Konfigurationen oder Einstellungen übergibst.
  - _Beispiel:_ `erstelle_user(name="Anna", rolle="Admin")`.

---

### Der Trick mit dem "Einpacken" vs. "Entpacken"

Das größte Verwirrspiel entsteht, weil die Sternchen zwei Gesichter haben – je nachdem, wo sie im Code stehen:

| Wo steht das Sternchen?                        | Bedeutung     | Was passiert?                            |
| :--------------------------------------------- | :------------ | :--------------------------------------- |
| **Im Funktionskopf** <br>`def func(*args):`    | **Einpacken** | Macht aus losen Werten ein Tupel.        |
| **Im Funktionsaufruf** <br>`func(*mein_tupel)` | **Entpacken** | Macht aus einem Tupel wieder lose Werte. |

> **Hinweis:** Genau das gleiche gilt für `**`: Im Kopf packt es Argumente in ein Dict, beim Aufruf (`func(**mein_dict)`) bricht es das Dict auf und übergibt die Werte als `key=value`.

```python
# 1. Die normale Funktion, die am Ende aufgerufen werden soll
def mein_profil(name, alter):
    print(f"--> Original-Funktion 'mein_profil' wurde aufgerufen mit: name='{name}', alter={alter}")
    return f"Erfolg! User {name} ist {alter} Jahre alt."

# 2. Der Decorator (genauso wie in deiner Übung)
def configure_plugin_decorator(func):
    def wrapper(*args):
        # Zeige, was *args als Tupel einsammelt
        print("\n[Schritt 1] *args hat das empfangen:", args)
        # [Schritt 1] *args hat das empfangen: (('name', 'Anna'), ('alter', 25))


        # Wandle das Tupel in ein Dictionary um
        new_dict = dict(args)
        print("[Schritt 2] dict(args) hat das daraus gemacht:", new_dict)
        # [Schritt 2] dict(args) hat das daraus gemacht: {'name': 'Anna', 'alter': 25}


        # Entpacke das Dictionary mit ** und rufe die Originalfunktion auf
        print("[Schritt 3] Rufe func(**new_dict) auf...")
        result = func(**new_dict)
        # print(result - Erfolg! User Anna ist 25 Jahre alt.
)

        return result
    return wrapper

# 3. Den Decorator auf die Funktion anwenden
verpacktes_profil = configure_plugin_decorator(mein_profil)

# 4. Den Wrapper testen (mit Tupeln als Argumente)
endergebnis = verpacktes_profil(("name", "Anna"), ("alter", 25))

print("\n[Endergebnis der Funktion]:", endergebnis)

"""
[Schritt 1] *args hat das empfangen: (('name', 'Anna'), ('alter', 25))
[Schritt 2] dict(args) hat das daraus gemacht: {'name': 'Anna', 'alter': 25}
[Schritt 3] Rufe func(**new_dict) auf...
--> Original-Funktion 'mein_profil' wurde aufgerufen mit: name='Anna', alter=25

[Endergebnis der Funktion]: Erfolg! User Anna ist 25 Jahre alt.
"""
```

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


any_var = vowel_counter(process_doc)
any_var("Something wicked this way comes")
"""
```
