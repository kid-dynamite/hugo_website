+++
date = '2026-09-13T11:30:32+02:00'
draft = false
title = 'Big_O Notation'
showAuthor = false
weight =11
layout = "simple"
summary = "🚀 Big_O Notation"
+++

## Big O Categories Review

| Big-O         | Name         | Description                                                                                                           |
| :------------ | :----------- | :-------------------------------------------------------------------------------------------------------------------- |
| `O(1)`        | Constant     | **Best.** Constant execution time regardless of data size. _Example:_ Array index lookup.                             |
| `O(log(n))`   | Logarithmic  | **Great.** Reduces steps per iteration; very fast for large datasets. _Example:_ Binary search.                       |
| `O(n)`        | Linear       | **Good.** Work scales directly with input size, typical for single loops. _Example:_ Unsorted array search.           |
| `O(n*log(n))` | Linearithmic | **Okay.** Slightly slower than linear. _Example:_ Mergesort.                                                          |
| `O(n^2)`      | Quadratic    | **Slow.** Work scales with the square of input size. _Example:_ Nested loops for pairs.                               |
| `O(n^3)`      | Cubic        | **Slower.** Scales with the cube of input size. _Example:_ Triple nested loops.                                       |
| `O(2^n)`      | Exponential  | **Horrible.** Avoid if possible; adding one input doubles steps. _Example:_ Brute-force coin flips.                   |
| `O(n!)`       | Factorial    | **Even More Horrible.** Extremely slow and practically unusable for large inputs. _Example:_ Generating permutations. |
