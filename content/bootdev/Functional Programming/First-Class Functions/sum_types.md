+++
date = '2026-09-01T17:53:47+02:00'
draft = false
title = 'Sum_types'
showAuthor = false
weight =55
layout = "simple"
summary = "🚀 sum_types"
+++

> Match

```python
from enum import Enum


class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3



def get_hex(color: Color) -> str:
    match color:
        case Color.RED:
            return "#FF0000"
        case Color.GREEN:
            return "#00FF00"
        case Color.BLUE:
            return "#0000FF"

        # default case (invalid Color)
        case _:
            return "#FFFFFF"



class Shade(Enum):
    LIGHT = 1
    DARK = 2


def get_hex(color: Color, shade: Shade) -> str:
    match (color, shade):
        case (Color.RED, Shade.LIGHT):
            return "#FFAAAA"
        case (Color.RED, Shade.DARK):
            return "#AA0000"
        case (Color.GREEN, Shade.LIGHT):
            return "#AAFFAA"
        case (Color.GREEN, Shade.DARK):
            return "#00AA00"
        case (Color.BLUE, Shade.LIGHT):
            return "#AAAAFF"
        case (Color.BLUE, Shade.DARK):
            return "#0000AA"

        # default case (invalid combination)
        case _:
            return "#FFFFFF"


```

> Match

```python
from enum import Enum


class DocFormat(Enum):
    PDF = 1
    TXT = 2
    MD = 3
    HTML = 4


def convert_format(
    content: str, from_format: DocFormat, to_format: DocFormat | None
) -> str:
    match (from_format, to_format):
        case (DocFormat.MD, DocFormat.HTML):
            return "<h1>" + content[2:] + "</h1>"

        case (DocFormat.TXT, DocFormat.PDF):
            return "[PDF] " + content + " [PDF]"

        case (DocFormat.HTML, DocFormat.MD):
            return "# " + content[4:-5]

        case _:
            raise Exception("invalid type")


# ==========================================
# HARDCODED TESTEINGABEN ZUM ABSPEICHERN
# ==========================================

# Test 1: Markdown zu HTML
print("--- Test 1 (MD zu HTML) ---")
eingabe_md = "# Hello, world!"
ergebnis_html = convert_format(eingabe_md, DocFormat.MD, DocFormat.HTML)
print(f"Eingabe:  {eingabe_md}")
print(f"Ergebnis: {ergebnis_html}\n")

# Test 2: Text zu PDF
print("--- Test 2 (TXT zu PDF) ---")
eingabe_txt = "This is plain text."
ergebnis_pdf = convert_format(eingabe_txt, DocFormat.TXT, DocFormat.PDF)
print(f"Eingabe:  {eingabe_txt}")
print(f"Ergebnis: {ergebnis_pdf}\n")

# Test 3: HTML zu Markdown
print("--- Test 3 (HTML zu MD) ---")
eingabe_html = "<h1>This is a heading</h1>"
ergebnis_md = convert_format(eingabe_html, DocFormat.HTML, DocFormat.MD)
print(f"Eingabe:  {eingabe_html}")
print(f"Ergebnis: {ergebnis_md}\n")

# Test 4: Fehlerfall (Exception)
print("--- Test 4 (Ungültiger Typ) ---")
try:
    # Ungültige Konvertierung von PDF zu TXT triggert die Exception
    convert_format("Test", DocFormat.PDF, DocFormat.TXT)
except Exception as e:
    print(f"Fehler erfolgreich abgefangen: {e}")
```

> Enums

```python
from enum import Enum

Color = Enum("Color", ["RED", "GREEN", "BLUE"])
print(Color.RED)  # this works, prints 'Color.RED'
print(Color.TEAL)  # this raises an exception

# New Code

from enum import Enum


class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3


print(Color.RED)  # this works, prints 'Color.RED'
print(Color.TEAL)  # this raises an exception

# New Code


def color_to_hex(color: Color) -> str:
    if color == Color.GREEN:
        return "#00FF00"
    elif color == Color.BLUE:
        return "#0000FF"
    elif color == Color.RED:
        return "#FF0000"
    # handle the case where the color is invalid
    raise Exception("unknown color")
```

> Union Types

```python
class Parsed:
    def __init__(self, doc_name: str, text: str) -> None:
        self.doc_name = doc_name
        self.text = text


class ParseError:
    def __init__(self, doc_name: str, err: str) -> None:
        self.doc_name = doc_name
        self.err = err


# Don't touch above this line


def parse_document(doc_name: str, content: str) -> Parsed | ParseError:
    if content:
        return Parsed(doc_name, content)
    else:
        return ParseError(doc_name, "no content")



def display_parse_result(result: Parsed | ParseError) -> str:
    if isinstance(result, Parsed):
        return f"Parsed {result.doc_name}: {len(result.text)} characters"
    else:
        return f"Failed {result.doc_name}: {result.err}"
```
