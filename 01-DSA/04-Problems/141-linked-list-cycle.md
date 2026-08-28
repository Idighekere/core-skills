---
number: "141"
title: 141-linked-list-cycle
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/linked-list
---

# 141-linked-list-cycle

**Link:** https://leetcode.com/problems/linked-list-cycle?envType=problem-list-v2&envId=linked-list

## Summary
Describe the problem in your own words.
The goal of the problem is to detect if a cycle exist in a linkedlist The idea from[Circular Linked List](Circular%20Linked%20List.md), 

A cycle exist when we continously traverse a linkedlist without encountering a tail or node's next to be `None`
## First thoughts
What was your initial idea?

I use the [Fast and Slow Pointers](Fast%20and%20Slow%20Pointers.md), approach, where the slow moves 1 step while the fast moves 2 steps. 
When we continue iterating at somepoint the slow and fast pointer will meet at a particular node. 


## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

1. initial the `slow` and `fast` to `head`
2. Move `slow` a step, while `fast` 2 steps
3. Since we aren't gonna reach the end, theres a point where both pointers will meet
4. return true when that happens
5. If there's no cycle, return `False`

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

## Learned
What transfers to other problems?

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month