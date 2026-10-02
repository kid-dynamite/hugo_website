+++
date = '2026-09-23T12:14:46+02:00'
draft = false
title = 'Hashmaps'
showAuthor = false
weight =15
layout = "simple"
summary = "🚀 Hashmaps"
+++

> Linear Probing

```python
from typing import Any


class HashMap:
    def insert(self, key: str, value: Any) -> None:
        index = self.key_to_index(key)
        original_index = index
        first_iteration = True

        # Schleife läuft, solange der Slot belegt ist UND der Key nicht übereinstimmt
        while self.hashmap[index] is not None and self.hashmap[index][0] != key:
            if not first_iteration and index == original_index:
                raise Exception("hashmap is full")

            # Zum nächsten Index springen und am Ende wieder von vorne anfangen (Modulo)
            index = (index + 1) % len(self.hashmap)
            first_iteration = False

        # Leeren Slot oder passenden Key gefunden -> Einfügen/Überschreiben
        self.hashmap[index] = (key, value)

    def get(self, key: str) -> Any:
        index = self.key_to_index(key)
        original_index = index
        first_iteration = True

        # Schleife läuft, solange der Slot belegt (nicht None) ist
        while self.hashmap[index] is not None:
            if self.hashmap[index][0] == key:
                return self.hashmap[index][1]

            if not first_iteration and index == original_index:
                raise Exception("sorry, key not found")

            # Zum nächsten Index springen und am Ende wieder von vorne anfangen
            index = (index + 1) % len(self.hashmap)
            first_iteration = False

        # Wenn wir auf ein None stoßen, existiert der Key in der Map nicht
        raise Exception("sorry, key not found")

    # don't touch below this line

    def __init__(self, size: int) -> None:
        self.hashmap: list[tuple[str, Any] | None] = [None for i in range(size)]

    def key_to_index(self, key: str) -> int:
        total = 0
        for c in key:
            total += ord(c)
        return total % len(self.hashmap)

    def __repr__(self) -> str:
        final = ""
        for i, v in enumerate(self.hashmap):
            if v is not None:
                final += f" - {v!s}\n"
        return final


```
