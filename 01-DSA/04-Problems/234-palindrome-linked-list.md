# Problem: 234. Palindrome Linked List

**Date:** 2026-08-27, 05:02

**Platform:** Leetcode
**Difficulty:** Easy
**Link:** https://leetcode.com/problems/palindrome-linked-list/description/?envType=problem-list-v2&envId=linked-list

---

## Problem Summary

Describe the problem in your own words.

A palindrome is a number that reads the same when written from the back or in reverse.

So we are just to check if a linked list is a palindrome. 
And if it can be done in O(n) time and O(1) space

---

## First Thoughts

What was your initial idea?

---

## Pattern(s) Used

- 

---

## Data Structure(s) Used

- 

---

## Key Insight

What realization unlocked the solution?

---

## Solution Strategy

Explain the approach step by step.

We need 3 core patterns:
1. Find the middle
2. Reverse the linkedlist from the slow and make the fast pointer to point to teh head
3. compare the reversed list (slow) with that of fast, if a number doesn't match return `False`, but after the checks and all number matches, return the `True`

Steps
1. If the list is empty or has a single item, return `True`
2.  We use the `fast` and `slow` pointers, so as to find the middle, where `slow` will point to the middle node
3. From the middle `slow` we reverse the list through to the end.
4. We set the `head` to be the head of the `left` list and the last node (which will be the  like the head of the `right` after the reversal)
5. After the reversal, we start comparing each value of the two `left` and `right` node, if at any point a node's value do not match, we return `False`, but after going through the both list.
6. We return `True`, becuase at this point we found both lists to match.


---

## Complexity

| Metric | Complexity |
| -------- | ---------- |
| Time | |
| Space | |

---

## Mistakes I Made

- 

---

## Edge Cases

- 

---

## What I Learned

- 

---

## Alternative Solutions

- 

---

## Similar Problems

- 

---

## Revisit

- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month