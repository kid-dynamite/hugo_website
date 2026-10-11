+++
date = '2026-10-02T19:16:17+02:00'
draft = false
title = 'Error handling & logging in Python'
showAuthor = false
weight = 20
layout = "simple"
summary = "🚀 error handling & logging in Python"
+++

> try & exceptions & finally & else

```python
# https://www.youtube.com/watch?v=ZUqGMDppEDs

import logging

try:
    print(10/0)
except:
    print("Error occured!")

print("Hello")

# Error occured!
# Hello



try:
    print(10/0)
except:
    pass
    # continue in for-loop

print("Hello")

# Hello



try:
    print(10/0)
except ZeroDivisionError:
    print("ZeroDivisionError Error occured!")

print("Hello")

# ZeroDivisionError Error occured!
# Hello



try:
    print(int("Hello"))
except ValueError as e:
    print(str(e))

print("Hello")

# invalid literal for int() with base 10: 'Hello'
# Hello



try:
    print(int("Hello"))
except Exception as e:
    print(str(e))

print("Hello")

# invalid literal for int() with base 10: 'Hello'
# Hello


try:
    print(int("Hello"))
except Exception as e:
    logging.exception(e)

print("Logging")

# ERROR Message
# Logging


try:
    print(int("Hello"))
except (AttributeError, ValueError) as e:
    pass
    #print(str(e))

print("Tuple")

# Tuple

#++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Raise Exception

try:
    #print(10/0)
    print(int("Alex"))

except ZeroDivisionError as e:
    pass
except ValueError as e:
    print("before raise command")
    raise
print("Hello")

# before raise command
# just an Error Message

#++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# finally: will always be executed at the end of the block
# else: will always be executed when there is no exception

try:
    print("Versuche Code auszuführen...")
except ValueError:
    print("Fehler aufgetreten!")
else:
    print("Erfolg! Kein Fehler aufgetreten.") # Läuft NUR, wenn alles glattging
finally:
    print("Ich laufe IMMER!") # Läuft, egal ob Fehler oder nicht


```
