---
title: "Prefix Sum"
date: 2026-08-28
type: pattern
tags: [pattern/prefix-sum]
---

# Prefix Sum

## Recognition clues
- **"Sum of a subarray / range"** repeated many times in one problem.
- Brute force is O(n) per query → the prefix array turns every query into O(1).
- "Running sum" phrasing.

## When to use
When you need **range sums** (`sum(a[l:r])`) over and over. Precompute once, answer instantly.

## Requirements
- The array doesn't change between queries (or you accept recomputation on update).

## Generic algorithm
```python
prefix = [0] * (len(nums) + 1)     # prefix[i] = sum of first i elements
for i, num in enumerate(nums):
    prefix[i + 1] = prefix[i] + num

# sum of nums[l:r] (inclusive l, exclusive r) == prefix[r] - prefix[l]
```
A running-sum result array is the same idea as its output:
```python
def running_sum(nums):
    total = 0
    res = []
    for num in nums:
        total += num
        res.append(total)
    return res
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | O(n) to build, O(1) per range query |
| Space | O(n) for the prefix array |

## Gotchas
- Off-by-one on the prefix array meaning (`prefix[i]` = first *i* elements, so a 0-length prefix is `prefix[0]=0`).
- Building a new result array when a running variable would do (O(n) space vs O(1)).

## Example problems
- [Running Sum of 1D Array](../04-Problems/1480-running-sum-of-1d-array.md)
- Subarray sum equals k
- Range sum queries (immutable)

## Similar patterns
- [Sliding Window](Sliding%20Window.md) — choose prefix-sum when you need *arbitrary* ranges, window when the range moves contiguously.
- [Hash Table](../01-Data%20Structures/Hash%20Table.md) — prefix sum + a hash map of "prefix seen so far" is the classic O(n) subarray-sum trick.