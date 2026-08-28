---
title: "Fast and Slow Pointers"
date: 2026-08-28
type: pattern
tags: [pattern/fast-and-slow]
---

# Fast and Slow Pointers

## Recognition clues
- Linked list, and the question mentions **middle**, **cycle**, **halves**, or **palindrome**.
- "Do it in O(1) space" — you may not allocate a secondary structure.
- Floyd's detection name-dropped in the problem or discussion.

## When to use
When walking a linked structure where a second pass would be wasteful: find the midpoint, detect a cycle, or split a list in half — in **one pass** and **constant space**.

## Requirements
- A single-linked (or doubly) traversable structure.

## Generic algorithm
```python
slow = fast = head
while fast is not None and fast.next is not None:
    slow = slow.next        # moves 1 step
    fast = fast.next.next   # moves 2 steps
# slow is now at the middle (or the meeting point if cycle)
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | O(n) — single pass |
| Space | O(1) — two pointers only |

## Variations
- **First vs second middle:** even-length lists have two middles. `while fast.next and fast.next.next` gives the *first* middle; `while fast and fast.next` gives the second-ish → make sure you know which to return.
- **Cycle detection (Floyd's):** add `if slow is fast: return True` inside the loop.
- **kth from end (offset pointers):** a *fixed-offset* variant — advance `fast` by k, then walk both; `slow` lands at the node before the target.

## Gotchas
- Forgetting the `fast is not None AND fast.next is not None` guard — crash on even-length lists / None tail.
- Using `==` instead of `is` for cycle detection (value equality can false-positive; identity is for the object).
- Returning the wrong middle for even lengths.

## Example problems
- Middle of linked list
- [Palindrome Linked List](../04-Problems/234-palindrome-linked-list.md) (find middle → reverse second half → compare)
- Linked list cycle / cycle start

## Similar patterns
- [Two Pointers](Two%20Pointers.md) — order from sorting; fast/slow gets order from *speed*.
- [Dummy Head](../01-Data%20Structures/Linked%20List.md) — often composed with fast/slow when the head may change.