+++
date = '2026-07-26T22:58:22+02:00'
draft = false
title = 'Recursion_lists'
showAuthor = false
weight =52
layout = "simple"
summary = "🚀 list recursion"
+++

```python
def sum_nums(nums: list[int]) -> int:
    if len(nums) == 0:
        return 0
    return nums[0] + sum_nums(nums[1:])


print(sum_nums([1, 2, 3, 4, 5]))        # prints: 15
```

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int the list
        ***Assume hte list is not empyt
        Ex: if lst = [9, 31,9], maxList(lst) returns 31
    """


    if len(lst) == 1:
        return lst[0]
    else:
        tempMax = maxList(lst[1:])
        return lst [0] if lst[0] > tempMax else tempMax
print(maxList([9, 31,9,7]))
```

```python
def maxList(lst):

    """
        Use Recursion to
        Return the maximum value int the list
        ***Assume hte list is not empyt
        Ex: if lst = [9, 31,9, "string", [99, 4, "er"]], maxList(lst) returns 99
    """

    if len(lst) == 0:
        return None
    else:
        tempMax = float("-inf")
        for item in lst:
            if isinstance(item, list):
                max_in_nested = maxList(item)
                if max_in_nested > tempMax:
                    tempMax = max_in_nested
            elif isinstance(item, int):
                if item > tempMax:
                    tempMax = item
        return tempMax if tempMax != float("-inf") else None

print(maxList([9, 31,9, "string", [99, 4, "er"]]))
```

```python
def maxList(lst):
    """
    Use Recursion to Return the maximum value in the list
    """
    if len(lst) == 0:
        return None
    else:
        tempMax = float("-inf")
        for item in lst:
            # Das if-Statement wird bei JEDEM Element als Erstes abgefragt:
            print(f"--> Abfrage 'if': Ist '{item}' eine Liste?")

            if isinstance(item, list):
                print(f"    JA! '{item}' ist eine Liste. Ich springe tiefer hinein...\n")
                max_in_nested = maxList(item)

                print(f"    <-- Zurück aus der Unterliste. Gefundenes Max dort war: {max_in_nested}")
                if max_in_nested > tempMax:
                    tempMax = max_in_nested

            elif isinstance(item, int):
                print(f"    NEIN! Aber '{item}' ist eine Zahl. Vergleiche mit tempMax...")
                if item > tempMax:
                    tempMax = item
                    print(f"    => Neues tempMax ist: {tempMax}")

        return tempMax if tempMax != float("-inf") else None

# Starte den Testlauf
print("=== START DES PROGRAMMS ===")
ergebnis = maxList([9, 31, 9, "string", [99, [201, [501]], 4, "er"]])
print("===========================")
print(f"Endergebnis: {ergebnis}")
```
