---
title: "Big-O Complexity"
date: 2026-08-28
type: concept
tags: [concept/complexity]
---

# Big-O Complexity

## Definition
Big-O describes **how runtime (or memory) grows as input size n grows** — not exact timing. It's the language every other note in this vault speaks: every operation table, every pattern, every problem cites it.

## The hierarchy (small → large)
```
O(1)    constant     — hash lookup, array access by index
O(log n) logarithmic — binary search (halving each step)
O(n)    linear       — single pass
O(n log n) linearithmic — efficient sorts (quicksort, mergesort)
O(n²)   quadratic    — nested loops over the same array
O(2ⁿ)   exponential  — naive subsets/permutations
O(n!)   factorial    — permutations
```

## Recognition rules of thumb
- A single loop over n → **O(n)**.
- A loop inside a loop over n → **O(n²)**.
- Halving the search space each step → **O(log n)**.
- "Two halves recursively" → **O(n log n)**.
- Loops that *don't* run over n (fixed k iterations) are **O(1)**.

Worked examples from Grokking Algorithms (Ch. 1):
- Find a name in a phone book → `O(log n)` (binary search, sorted by name).
- Find a name from a phone *number* → O(n) (numbers aren't sorted).
- Read the numbers of everyone in the book → O(n).
- Read the numbers of *just the As* → still **O(n)** — 1/26th of n is `(1/26)·n`, and constants drop out of Big-O.
- Double the value of the first element → O(1).
- Double every element → O(n).
- Multiplication table over n elements → O(n²).

## Gotchas
- **Constants are dropped.** `1/26 · n`, `2n`, `n + 1000` are all O(n). What matters is the *shape* as n grows.
- **Worst vs average** must be explicit: hash tables are O(1) *average*, O(n) *worst* (collisions). Quicksort is O(n log n) average, O(n²) worst (bad pivot).
- **Space complexity** gets forgotten. Note both time and space in every solution.
- Doubling the input: O(n) → 2× time; O(n²) → 4× time; O(log n) → 1 more step. This intuition catches design mistakes fast.

## Related
- [Binary Search](Binary%20Search.md) — the O(log n) poster child.
- [Sorting Algorithms](Sorting%20Algorithms.md) — where O(n log n) comes from.