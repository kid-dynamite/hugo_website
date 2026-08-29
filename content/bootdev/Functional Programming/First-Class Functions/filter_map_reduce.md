+++
date = '2026-07-31T21:56:44+02:00'
draft = false
title = 'Lambda, Filter, Map, Reduce'
showAuthor = false
weight =50
layout = "simple"
summary = "🚀 simple examples of lambda, filter and reduce"
+++

```python
from functools import reduce

#def is_even(n):
    #return n%2 == 0

nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

#evens = list(filter(is_even, nums))
evens = list(filter(lambda n : n%2 == 0, nums))

doubles = list(map(lambda n : n*2, evens))

sum = reduce(lambda a, b : a+b, doubles)

print(evens)    # [2, 4, 6, 8, 10]

print(doubles)  # [4, 8, 12, 16, 20]


print(sum)      # 60


```

> map()

```python
def change_bullet_style(document: str) -> str:


    lines_list = document.split("\n")
    # ['- Bonzo Madrid', '- Stilson', '- The Formics', '- Peter Wiggin', '- Valentine Wiggin', '- Colonel Graff', '']
    docs = list(map(convert_line, lines_list))
    rejoined_doc = "\n".join(docs)
    return rejoined_doc


# Don't edit below this line


def convert_line(line: str) -> str:
    old_bullet = "-"
    new_bullet = "*"
    if len(line) > 0 and line[0] == old_bullet:
        return new_bullet + line[1:]
    return line

print(change_bullet_style("- Bonzo Madrid\n- Stilson\n- The Formics\n- Peter Wiggin\n- Valentine Wiggin\n- Colonel Graff\n"))

"""
daten = "Apfel,Banane,Kirsche,Mango"
fruechte = daten.split(",")

print(fruechte)
# Ausgabe: ['Apfel', 'Banane', 'Kirsche', 'Mango']

***********************************************************************

fruechte = ['Apfel', 'Banane', 'Kirsche']

# Mit Komma und Leerzeichen verbinden
csv_text = ", ".join(fruechte)
print(csv_text)
# Ausgabe: Apfel, Banane, Kirsche

# Ohne Trennzeichen direkt aneinanderhängen
zusammen = "".join(fruechte)
print(zusammen)
# Ausgabe: ApfelBananeKirsche
"""

```

> filter()

```python
def remove_invalid_lines(document: str) -> str:

    split_str = document.split("\n")
    print(split_str) #['', '* We are the music makers', '- And we are the dreamers of dreams', "* Come with me and you'll be", '']
    doc = list(filter(lambda line : not line.startswith("-"), split_str))
    rejoined_doc = "\n".join(doc)
    return rejoined_doc


print(remove_invalid_lines("\n* We are the music makers\n- And we are the dreamers of dreams\n* Come with me and you'll be\n",))

"""
* We are the music makers
* Come with me and you'll be
"""


```

> reduce()

```python

import functools


def join(doc_so_far: str, sentence: str) -> str:
    joined = ". ".join([doc_so_far, sentence])
    print(joined)

    #You wrote a bad song. This is a good idea
    #You wrote a bad song. This is a good idea. Just buy the tree

    return joined



def join_first_sentences(sentences: list[str], n: int) -> str:
    if not n:
        empty_str = ""
        return empty_str
    sliced_sentence_list = sentences[:n]
    # ['You wrote a bad song', 'This is a good idea', 'Just buy the tree']

    final_str = functools.reduce(join, sliced_sentence_list)
    return final_str + "."


test_list =  [
            "You wrote a bad song",
            "This is a good idea",
            "Just buy the tree",
            "It's going to flood",
            "Tell us what to do",

        ]

print(join_first_sentences(test_list, 3))


```

> zip()

```python
namen = ["Alice", "Bob", "Charlie"]
berufe = ["Developer", "Designer", "Manager"]

# Jetzt der Reißverschluss:
ergebnis = list(zip(namen, berufe))
print(ergebnis)
# [('Alice', 'Developer'), ('Bob', 'Designer'), ('Charlie', 'Manager')]


# ***********************************************************************


namen = ["Alice", "Bob", "Charlie", "Daniel"]  # 4 Elemente
berufe = ["Developer", "Designer"]             # 2 Elemente

ergebnis = list(zip(namen, berufe))
print(ergebnis)
# Ausgabe: [('Alice', 'Developer'), ('Bob', 'Designer')]
# (Charlie und Daniel fallen weg!)


# ***********************************************************************


namen = ["Alice", "Bob"]
punkte = [100, 85]

for name, score in zip(namen, punkte):
    print(f"{name} hat {score} Punkte erreicht.")

# Alice hat 100 Punkte erreicht.
# Bob hat 85 Punkte erreicht.


# ***********************************************************************



heroes = ["Gimli", "Legolas"]
dmg = [150, 220]

# 1. Leere Liste erstellen
stats = []

for hero, damage in zip(heroes, dmg):
    # 2. Text in die Liste packen
    stats.append(f"{hero} fügt {damage} Schaden zu.")

# Jetzt hast du eine fertige Liste, die du weitergeben kannst
print(stats)
#['Gimli fügt 150 Schaden zu.', 'Legolas fügt 220 Schaden zu.']


# List compehension

stats = [f"{hero} fügt {damage} Schaden zu." for hero, damage in zip(heroes, dmg)]
print(stats)

```

> Higher-Order Functions

```python
def restore_documents(originals: tuple[str, ...], backups: tuple[str, ...]) -> set[str]:
    #map_both = list(map(lambda x : x.upper(), originals + backups))
    #filter_out = list(filter(lambda x : not x.isdigit(), map_both))
    map_filter = list(filter(lambda x : not x.isdigit(), list(map(lambda x : x.upper(), originals + backups))))

    return set(map_filter)


a = ("Mortgage", "Marriage Certificate", "Boot.dev Certificate")
b = ("VEHICLE TITLE", "1235023451345", "MORTGAGE")

print(restore_documents(a,b))
# {'MORTGAGE', 'BOOT.DEV CERTIFICATE', 'MARRIAGE CERTIFICATE', 'VEHICLE TITLE'}
# removed duplicates and digits. all in uppercase

```

> No-Op

```python
doc: str = """I *love* Markdown.
I **really love** Markdown.
I ***really really love*** Markdown."""


def remove_emphasis(doc: str) -> str:

    lines = doc.split("\n")
    print(f"lines: {lines}")
    new_lines = map(remove_line_emphasis, lines)
    new_doc = "\n".join(new_lines)
    return new_doc


# Don't touch below this line


def remove_line_emphasis(line: str) -> str:
    words = line.split()
    print(f"words: {words}")
    new_words = map(remove_word_emphasis, words)
    return " ".join(new_words)


def remove_word_emphasis(word: str) -> str:
    return word.strip("*")

print(f"Result: {remove_emphasis(doc)}")

"""
lines: ['I *love* Markdown.', 'I **really love** Markdown.', 'I ***really really love*** Markdown.']
words: ['I', '*love*', 'Markdown.']
words: ['I', '**really', 'love**', 'Markdown.']
words: ['I', '***really', 'really', 'love***', 'Markdown.']
Result: I love Markdown.
I really love Markdown.
I really really love Markdown.
"""
```
