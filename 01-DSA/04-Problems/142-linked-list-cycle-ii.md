---
number: "142"
title: 142-linked-list-cycle-ii
platform: leetcode
difficulty: medium
status: new
date: 2026-08-28
tags:
  - ds/linked-list
  - ds/hash-table
---
# 142-linked-list-cycle-ii

**Link:** https://leetcode.com/problems/linked-list-cycle-ii/description/?envType=problem-list-v2&envId=linked-list

## Summary
Describe the problem in your own words.

Previously in [141-linked-list-cycle](141-linked-list-cycle.md), the task was to detect if a linkedlist has a cycle, i.e the tail links to another node we have visited before and therefore not `None`, 

In this problem we are tasked to find out that node where the cycle began
## First thoughts
What was your initial idea?

Immediately i saw this problem, [Hash Table](Hash%20Table.md), came into my mind😅😅

I will just store the memory address of every node i visit in a hash set, especially since the address is gonna be unique. 

Then do the number traversal the moment I visit the node i might have seen before, it means there's a `cycle` and then i return the `node`

My approach is gonna have a **O(n)** space complexity and I read that there's a better way to solve this without introducing a hashset.

## Key insight
What realization unlocked the solution?

Realizing that i need a hashmap
## Solution
Explain the approach step by step.

I will use the [Fast and Slow Pointers](Fast%20and%20Slow%20Pointers.md), approach to traverse the list, the moment 
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