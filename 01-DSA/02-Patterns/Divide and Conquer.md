---
title: "Divide and Conquer"
date: 2026-08-28
type: pattern
tags: [pattern/divide-and-conquer]
---

# Divide and Conquer

## Recognition clues
- The problem is **self-similar**: solve a smaller version, then combine.
- Sorting / searching / finding extremes, where halving the input each step is natural (binary search, mergesort, quicksort).
- Recursive structure in a problem statement.

## When to use
When a problem can be split into smaller *independent* subproblems whose solutions combine into the full answer — and the split reduces the work per level (typically O(log n) levels or balanced partitions).

## Requirements
- A clear **base case** for the smallest input.
- A **recursive case** that shrinks the input toward the base.
- Combine step cheap enough to not dominate.

## Generic algorithm
```python
def solve(problem):
    if base_case(problem):
        return answer(problem)
    left, right = split(problem)        # divide
    return combine(solve(left), solve(right))   # conquer + combine
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | Depends on split/combine; classic: O(n log n) |
| Space | O(log n) recursion stack (balanced splits) |

## Variations
- **Quicksort** — pick pivot, partition, recurse on halves:
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    less = [x for x in arr[1:] if x < pivot]
    greater = [x for x in arr[1:] if x >= pivot]
    return quick_sort(less) + [pivot] + quick_sort(greater)
```
- **Binary search** — halve, discard the half that cannot contain the target (see [Binary Search](../03-Concepts/Binary%20Search.md)). Its base case is "found" or "range empty".
- **Recursion in general** — D&C *is* the recursion pattern applied to "split then combine" (see [Recursion](../03-Concepts/Recursion.md)).

## Gotchas
- Forgetting a base case → infinite recursion / stack overflow.
- Pivot choice killing the balance → worst case O(n²) for quicksort on already-sorted input.
- Counting recursion depth: each halving adds a stack frame.

## Example problems
- Quicksort / mergesort
- Finding max of a list recursively
- Binary search

## Similar patterns
- [Recursion](../03-Concepts/Recursion.md) — the umbrella concept.
- [Binary Search](../03-Concepts/Binary%20Search.md) — the narrowest, most perf-critical D&C.
- [Breadth-First Search (BFS)](Breadth-First%20Search%20%28BFS%29.md) — the *other* major traversal family (iterative, level-based, not recursive-dividing).