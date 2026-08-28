---
title: "Sorting Algorithms"
date: 2026-08-28
type: concept
tags: [concept/sorting]
---

# Sorting Algorithms

## Why sorting matters
Sorted input unlocks almost every efficient pattern: [Binary Search](Binary%20Search.md), [Two Pointers](../02-Patterns/Two%20Pointers.md), [Sliding Window](../02-Patterns/Sliding%20Window.md), merging. "Sort once, answer many" is a recurring interview move.

## Selection sort — the naive one (O(n²))
Find the smallest, put it aside, repeat. Simple but quadratic — fine to demonstrate, never the choice in practice.

```python
def find_smallest(arr):
    smallest, smallest_index = arr[0], 0
    for i in range(1, len(arr)):
        if arr[i] < smallest:
            smallest, smallest_index = arr[i], i
    return smallest_index

def selection_sort(arr):
    result, copy = [], list(arr)
    while copy:
        i = find_smallest(copy)
        result.append(copy.pop(i))
    return result
```

## Quicksort — D&C in action (O(n log n) average)
Pick a pivot, partition into *less* and *greater*, recurse on both.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    less    = [x for x in arr[1:] if x < pivot]
    greater = [x for x in arr[1:] if x >= pivot]
    return quick_sort(less) + [pivot] + quick_sort(greater)
```
Worst case O(n²) when the pivot is consistently bad (e.g., sorted input with a fixed first-element pivot).

## The quick table

| Algorithm | Best | Average | Worst | Notes |
|-----------|------|---------|-------|-------|
| Selection sort | O(n²) | O(n²) | O(n²) | trivial, stable-ish, never use |
| Quicksort | O(n log n) | O(n log n) | O(n²) | bad pivot → quadratic; cache-friendly |
| Mergesort | O(n log n) | O(n log n) | O(n log n) | stable, extra O(n) space |
| Timsort (Python's `sorted`) | O(n) | O(n log n) | O(n log n) | hybrid; just use `sorted()` |

## Python reality
In interviews and notebooks: **use `sorted()` / `.sort()`**. Know the theory to reason about *why* n log n matters and when merge's stability or quicksort's space saving wins — don't hand-write a sort unless the problem is literally "implement sorting".

## Gotchas
- Selecting a sort by its *average* when worst-case input is realistic (quicksort).
- Forgetting Python's sort is stable (equal elements keep relative order) — relevant to multi-key sorts.

## Related
- [Divide and Conquer](../02-Patterns/Divide%20and%20Conquer.md) — quicksort's pattern.
- [Big-O Complexity](Big-O%20Complexity.md).