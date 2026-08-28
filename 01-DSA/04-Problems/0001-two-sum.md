---
number: "1"
title: 0001-two-sum
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/array
  - ds/hash-table
---

# 0001-two-sum

**Link:** https://leetcode.com/problems/two-sum/

## Summary
Describe the problem in your own words.

## First thoughts
What was your initial idea?

I initially thought of adding the value of an index with the value of the next index and comparing with the target. i return the indexes if they sum adds up to the target.

This will require usig two nested loops(for), the outer for the i-index(first number) and the second with the j-index

## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

The implementation of my previous intuition requires two nested loop.

1. the inner loop starts from the next index after the outer loop like i+1,
2. then i do a condition to return the indexes (i and j) if the summation of array[i]+array[j] is the target.

The optimal approach is to use a hashmap and loop through the array once.

1. I subtract the taraget from the current value and see if the difference is already in hashmap.
2. If in hashmap. i return the index of the curent number and the index of the difference
3. If not in hashmap. I add the index as value and the value to the hashmap

For [2,7,11,15] where target is 9.

- I do 9-2 which is 7.
- I check if in hashmap, which won't initially be.
- Then I add 2 as key of hashmap and 0 as the value using hashmap[num]=1
- When current is 7, the process is repeated
- 9-7 =2
- I check hashmap if there is 2 and yes, it was added.
- I return it's value on the hashmap(the index of 2 in the array), with the value(index) of the current number

- **Time Complexity:** O(n^2) for my initially thought and O(n) when looping once
- **Space Complexity:** O(n).

```python
def BF_two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i]+nums[j]==target:
                return [i,j]

def two_sum(nums,target):
    hashmap={}

    for i, num in enumerate(nums):
        desired=target-num

        if desired in hashmap:
            return [i,hashmap[desired]]
        hashmap[num]=i

arr=[2,7,11,15]
print(two_sum(arr,9))
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