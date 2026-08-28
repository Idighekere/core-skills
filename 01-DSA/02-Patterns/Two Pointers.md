---
title: "Two Pointers"
date: 2026-08-28
type: pattern
tags: [pattern/two-pointers]
---

# Two Pointers

## Recognition clues
- The input is (or can be) **sorted** / ordered in some way.
- You want a **pair** (or two distinct elements) that satisfy a condition.
- You keep returning to the same brute force: nested loops over two indices.
- Keywords: pairs, triples (three pointers), merge two sorted lists, "don't use extra space".

## When to use
The punchline is *"you don't need to re-walk the collection for every element"*. One pointer walks from the front, another from the back or from the second list, and each step eliminates candidates — so O(n²) nested scans collapse to O(n).

## Requirements
- A definition of **order** between elements you're comparing (sorted array, two sorted lists).
- No need to look *back* more than the other pointer gives you (if you need arbitrary history, consider a [Hash Table](../01-Data%20Structures/Hash%20Table.md)).

## Generic algorithm
```python
# Merge two sorted structures / find pairs in a sorted array
i, j = 0, len(arr) - 1          # or 0, 0 across two lists
while i < j:                    # or while i < len(a) and j < len(b)
    if condition(i, j):
        i += 1                  # eliminate the left candidate
    else:
        j -= 1                  # eliminate the right candidate
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | O(n) per pass (vs O(n²) naive) |
| Space | O(1) — *no extra structures* |

## Variations
- **Opposite ends** (sorted array): two-sum-with-sorted-array. Each step shrinks the window by one.
- **Merge two lists**: keep a tail/dummy pointer and advance whichever list has the smaller head.
- **Three pointers:** i, j, k for triplets.

## Gotchas
- Off-by-one on the stop condition — `i < j` vs `i <= j` (must `i != j` for "two distinct").
- Both pointers on the *same list* vs two different lists — the merge variant stops when either list runs out, then *appends the remainder* (this is the silently-missed step).

## Example problems
- [Merge Two Sorted Lists](../04-Problems/merge-two-sorted-lists.md)
- [Palindrome Linked List](../04-Problems/234-palindrome-linked-list.md)
- [Two Sum](../04-Problems/0001-two-sum.md) (uses a hash table, but "complement" pairing is the same instinct)

## Similar patterns
- [Fast and Slow Pointers](Fast%20and%20Slow%20Pointers.md) — *same* idea, one pointer moves 2×; use when the structure is a linked list / cycle and order comes from the traverse speed, not from sorting.
- [Sliding Window](Sliding%20Window.md) — a *contiguous* window over one array; two pointers define the boundaries, but the “window” is the insight.