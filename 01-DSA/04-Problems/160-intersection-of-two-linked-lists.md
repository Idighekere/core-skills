---
number: "160"
title: 160-intersection-of-two-linked-lists
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/hash-table
  - ds/linked-list
---

# 160-intersection-of-two-linked-lists

**Link:** https://leetcode.com/problems/intersection-of-two-linked-lists/?envType=problem-list-v2&envId=linked-list

## Summary
Describe the problem in your own words.

## First thoughts
What was your initial idea?

[Hash Table](Hash%20Table.md) to the rescure again. 
Create a hashset `setA` and store every nodes in the first list `headA`, 
then traverse the second list, `headB` and return the `node` that is found in the set.

This  approach trades for more space, as I introduce a hashset to store every single node of a list. 
The space complexity is **O(m)** where **m** is the number of nodes in the first list.
The time complexity with this appraoch is **O(m+n)** where **m** and **n** are the number of the noes in the first and second list respectively..
## Solution
Explain the approach step by step.

I believe that this approach is not optimal that it can be solved in O(1) space complexity, but i haven't figured this out yet

## Mistakes
What I got wrong the first time.

I don't know what gave me the thought of using two hashset for the two heads. 

I would have simply, create single hashset for the first list and then traverse the second list and return the node when i find a value that is already in the set.

By using `setA` and `setB` and then try to loop through the set and check if a node in `setA` exists in `setB`, i will hit a critical bug as **"this approach will just return the very first node that shares the same value, even if it's not the actual node intersection**

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

## Learned
What transfers to other problems?

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month