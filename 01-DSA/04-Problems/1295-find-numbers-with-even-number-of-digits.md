---
number: "1295"
title: 1295-find-numbers-with-even-number-of-digits
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/array
---

# 1295-find-numbers-with-even-number-of-digits

**Link:** https://leetcode.com/problems/find-numbers-with-even-number-of-digits/

## Summary
Describe the problem in your own words.

## First thoughts
What was your initial idea?

I first thought o using a nested loop so i can get the count of numbersof each element in the array. then i then decided to use `str` and `len`. I think this mitght be a cheatsheet and might not be allowed.

## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

I have to go through every single and then to get the number of digits i convert each number to a str and then use the `len` method to find the length of it. I compare

1. Iterate over every element
2. Use the `str` and `len` to get the number of digits of each elements.
3. Then use the modulo operator to check if there's a remainder if dividing the length by 2.
4. IF there's a remainder i.e odd, just continue with the iteration
5. if there's no remainder, that's even then i increment the `count` variable which i initialised earlier on.
6. At the end i return the `count`

- **Time Complexity:** O(n), becuase i loop over every item once
- **Space Complexity:** O(1)

```python
def find_numbers(nums):
    count=0

    for num in nums:
        if len(str(num)) % 2 == 0:
            count+=1
        else:
            continue
    return count

arr=[12,345,2,6,7896]
arr2=[555,901,482,1771]
print(find_numbers(arr2))
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