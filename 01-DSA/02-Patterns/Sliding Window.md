---
title: "Sliding Window"
date: 2026-08-28
type: pattern
tags: [pattern/sliding-window]
---

# Sliding Window

## Recognition clues
- **Subarray / substring / contiguous segment** phrasing: "longest", "shortest", "contains all", "k elements".
- A window of fixed or variable size that you slide across the array.
- Brute force is O(n·k) (for every start, scan k) — the window makes it O(n).

## When to use
When the problem asks about *contiguous* ranges and you can decide to extend or shrink the range by one element at a time, keeping a running "answer state" (sum, counts, min…) instead of recomputing.

## Requirements
- The range must be **contiguous**.
- The window state must be **incrementally updatable** (add one on the right, subtract one from the left).

## Generic algorithm
```python
# Fixed-size window (e.g. best of every k)
state = initial_window
for i in range(k, len(arr)):
    state -= arr[i - k]     # drop the left
    state += arr[i]         # add the right
    update(answer)

# Shrinking/expanding window
l = 0
for r in range(len(arr)):   # r = right edge grows
    add arr[r] to state
    while condition_violated(state):
        remove arr[l]; l += 1
    update(answer)
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | O(n) — each element enters and leaves once |
| Space | O(1) or O(frequency-map) |

## Gotchas
- **Fixed vs variable window confusion.** Identify which one the problem needs.
- Forgetting to shrink (variable window) before updating the answer.
- Sliding by one when the window should jump.

## Example problems
- [Best Time to Buy and Sell Stock](../04-Problems/0121-best-time-to-buy-and-sell-stock.md) — technically a "carry the min across" window
- Longest substring without repeating characters
- Maximum sum of any subarray of size k

## Similar patterns
- [Two Pointers](Two%20Pointers.md) — the window *is* two pointers on the same array; the distinction is the contiguous-segment framing.
- [Prefix Sum](Prefix%20Sum.md) — an alternative for "sum of subarray" when the array doesn't shrink/grow well.