---
number: ""
title: 237-delete-node-in-a-linked-list
platform: ""
difficulty: ""
status: new
date: 2026-08-28
tags:
  - ds/linked-list
---

# 237-delete-node-in-a-linked-list

**Link:** https://leetcode.com/problems/delete-node-in-a-linked-list/description/?envType=problem-list-v2&envId=linked-list

## Summary
Describe the problem in your own words.

I am to delete a node in a linkedlist given only the node, I don't have access to the `head` to traverse to that node to delte and keep track of `prev` so the rewiring can be done. 

The problem guarantees that:
- the values are unique
- the given node is not the last node

By deleting we are not to remove it from memory, it should not just exist in the linkedlist again

## First thoughts
What was your initial idea?

I really thought of ways to reach this node without traversing from the `head` since i don't have access it. I couldn't figure it out myself after some tries, so i sought help from Youtube and watched solutions. 

## Key insight
What realization unlocked the solution?

😅😅😅 I watched the solution after i couldn't figure out. 
But I realize that since we have the `node` and not the `prev` to conduct the rewiring, we can just rewrite the node with it's next value and reset the next value to `None`

## Solution
Explain the approach step by step.

We have the `node` in question. so rewrite the value with it's `next`'s value and set the `next` value to `None`

## Mistakes
What I got wrong the first time.

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

We were told that values are unique, and we won't be given the `tail` to delete, so no much edge case to 
## Learned
What transfers to other problems?

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month