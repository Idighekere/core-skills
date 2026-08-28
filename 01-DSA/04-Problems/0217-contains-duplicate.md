---
number: "217"
title: 0217-contains-duplicate
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/hash-set
---

# 0217-contains-duplicate

**Link:** https://leetcode.com/problems/contains-duplicate/

## Summary
Describe the problem in your own words.

## First thoughts
What was your initial idea?

To find if any value appears at least twice, we need a way to "remember" the numbers we've already seen. A Hash Set is perfect for this because it allows for O(1) average time complexity for both insertions and lookups.

## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

1. Initialize an empty set.
2. Iterate through each number in the array.
3. If the number is already in the set, we found a duplicate; return `True`.
4. Otherwise, add the number to the set.
5. If we finish the loop without finding a duplicate, return `False`.

- **Time Complexity:** O(n) - We traverse the array once.
- **Space Complexity:** O(n) - In the worst case (no duplicates), we store all $n$ elements in the set.

```python
def check_duplicate(nums):

    hash=set()

    for num in nums:
        if num in hash:
            return True
        hash.add(num)
    return False


print(check_duplicate([1,2,3,1]))
```

## Mistakes
What I got wrong the first time.

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

## Learned
What transfers to other problems?

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month