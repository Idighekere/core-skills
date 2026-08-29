---
number: "19"
title: 19-remove-nth-from-end-of-list
platform: leetcode
difficulty: medium
status: new
date: 2026-08-28
tags:
  - ds/linked-list
---

# 19-remove-nth-from-end-of-list

**Link:** https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/?envType=problem-list-v2&envId=linked-list

## Summary
Describe the problem in your own words.

We will be given a position, and we are to remove the node at that position from the back.

say we are given `1 -> 3 -> 4` and we are given `n = 2`, it means from the end we count 2 and it lands at 3, so we remove node 3 from the list to have `1 -> 4`

The problem guarantees than n is always <= number of nodes in the [Linked List](Linked%20List.md)
## First thoughts
What was your initial idea?

Tbh, nothing much, I looked up youtube for the solution.

## Key insight
What realization unlocked the solution?

This is a [Fast and Slow Pointers](Fast%20and%20Slow%20Pointers.md) problem, by initially moving `fast` **n** steps ahead
When we start the iteration with `slow` starting from the head and `fast` a distance `n` steps ahead, we will findout that when `fast` reaches the end, `slow` will stop immediately at the node before the one we want to delete
 
 ## Solution
Explain the approach step by step.

This will require a **dummy head** so we don't have to handle edge cases where we have to remove the first node.

1. Start the `slow` and `fast` at `head`
2. move `fast` **n** steps ahead of `slow`
3. move both `slow` and `fast` at the  same pace
4. when `fast` reaches the end, `slow` will be at the node before the one we want to eliminate
5. rewire it's next to point to the next's next

```python
  def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode()
        dummy.next = head
        slow = fast = dummy

        for _ in range(n):
            fast = fast.next
        
        while fast.next is not None:
            slow = slow.next
            fast = fast.next
        
        slow.next = slow.next.next
    
        return dummy.next
```
## Mistakes
What I got wrong the first time.

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

- a single item, should return None, becuase n will be 1 and then it will remove that single item
- we will never be given an empty list
## Learned
What transfers to other problems?

The fast pointer must not always move 2 steps ahead of the slow pointer in the case of finding the middle

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month