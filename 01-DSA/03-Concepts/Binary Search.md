---
title: "Binary Search"
date: 2026-08-28
type: concept
tags: [concept/binary-search]
---

# Binary Search

## Definition
Search a **sorted** collection by repeatedly halving it: guess the middle, discard the half that can't contain the target. O(log n) instead of O(n) linear scan.

## Intuition
Word-guessing game. "Is it after M?" halves the dictionary every answer. With 128 names you need at most 7 guesses (log₂128 = 7); doubling to 256 names adds *exactly one* more guess.

## The algorithm
```python
def binary_search(arr, item):
    low, high = 0, len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        guess = arr[mid]
        if guess == item:
            return mid
        elif guess > item:
            high = mid - 1
        else:
            low = mid + 1
    return None
```

## Requirements
- **Sorted** input — binary search needs *random access* to the middle instantly, which is why:
  - Arrays support it (O(1) index access).
  - [Linked Lists](../01-Data%20Structures/Linked%20List.md) do **not** (getting the middle is O(n) — you'd walk every time).
- A total order on the values (comparability).

## Why it's O(log n)
Each step discards half: `n → n/2 → n/4 → … → 1`. Halving until one element remains takes `log₂n` steps. Doubling the list adds one step.

## The broader idea (helps interviews)
Binary search is a *decision-per-half* framework. The "sorted array" version is one flavor; the same halving logic applies to:
- "Find the boundary where condition(x) flips" (e.g., first bad version).
- Search space = the possible *answers* (minimize largest subarray sum).

## Gotchas
- Unsorted input → silently wrong answers.
- **Overflow** risk on `(low + high)` in languages with fixed ints — use `low + (high - low)//2`.
- Off-by-one on the loop condition: `low <= high` vs `low < high`, and `mid - 1`/`mid + 1` (else infinite loop when `low == high == mid`).

## Related
- [Divide and Conquer](../02-Patterns/Divide%20and%20Conquer.md) — the pattern family it belongs to.
- [Array](../01-Data%20Structures/Array.md), [Big-O Complexity](Big-O%20Complexity.md).