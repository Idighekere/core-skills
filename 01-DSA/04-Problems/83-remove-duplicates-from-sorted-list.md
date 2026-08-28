# Problem: 83. Remove Duplicates from Sorted List

**Date:** 2026-08-25, 17:27

**Platform:** Leetcode
**Difficulty:** Easy
**Link:** https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/?envType=problem-list-v2&envId=linked-list

---

## Problem Summary

Describe the problem in your own words.

We are provided with a list that is guaranteed to be always sorteed and we are to remove all duplicates and still return the unique lists still sorted

![Remove Dupllicates from sorted List](../../_attachments/Remove%20Dupllicates%20from%20sorted%20List%2020260826164942.png)

---

## First Thoughts

What was your initial idea?

I itereate over the linkedlists, and then compare the current node with the immediate next, if they are same I wil rewire it's pointer to point to the next's node next's eliminating its immediate next.

I tried this, but it won't handle cases where the duplicates are more than 2. So i tried using `prev` to compare the previous value with the curr, but it's still not efficient when the duplicates values are standing alone like `[1, 1, 1]`

```python
# Definition for singly-linked list.

# class ListNode:
	# def __init__(self, val=0, next=None):
		# self.val = val
		# self.next = next

	class Solution:
	
		def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
		
		cur = head
		
		if cur is None:
			return None

		prev = None
		
		while cur and cur.next is not None:
			if cur.val == cur.next.val:
				cur.next = cur.next.next
					
			if prev and prev.val == cur.val:
				prev.next = cur.next
				prev = cur
				# cur=cur.next
			
			cur = cur.next
		
		return head
```



---

## Data Structure(s) Used

- [Linked List](../01-Data%20Structures/Linked%20List.md)

---

## Key Insight

What realization unlocked the solution?

The duplicates can be more than one so when we rewire a node's next to be the node next's next, we shoulnd't move the pointer till we confirm that where we are rewiring to is not a duplicate.

---

## Solution Strategy

Explain the approach step by step.

We start my setting head to be `cur` so we can use it for iterataion 
1. we go iterate over the linkedlist and do this as long as `cur` and `cur.next` are valid nodes (not `None)
2. we compare `cur` with `cur.next` if they are same, we set `cur.next = cur.next.next`, this just makes the current node to point to the immediate node's next node thereby eliminating the immediate next node. 
3. The node we are rewiring to might be a duplicate so we need to rewire again, so to achieve, we make sure we don't increment `cur` until we meet a non duplicate node. 
4. at the end, we return the `head`

```python
class Solution:
	def deleteDuplicates(self, head:Optional[ListNode]) -> Optional[ListNode]:
		cur = head
		
		if cur is None
			return cur
		
		while cur:
			if cur.next and cur.next.val == cur.val:
				cur.next = cur.next.next
			else:
				cur = cur.next
			
		return head
```
---

## Complexity

We are just iterating over the lists once and not using extra variable to hold extra stuff.

Assuming we created an output variable to store non duplicate and compare it with the actaul lists, this would have made it O(n) space..

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

## Mistakes I Made

- I brought `prev` thinking i need it to check the previous node and the current after iterating but that didn't handle all each cases like in cases where all duplicates are same as in `[1, 1, 1]` 

---

## Edge Cases

-  When the duplicates are more than 2. This can be tackled by not just stopping after eliminating the immediate next as teh next's next we are rewring to might be a duplicate. 

---

## What I Learned

- Do not overthink
- 

---

## Alternative Solutions

**Recursive Approach**
- The recursive approach will just be to use recursion to elimate the duplicate or move to the next instead of using a loop. 

[Drawing (excalidraw)](../../_attachments/Drawing%202026-08-26%2017.31.56.excalidraw.md)

```python

class Solution:
	def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
	
		if head is None:
			return None
		
		if head.next and head.val == head.next.val:
			head = self.deleteDuplicates(head.next)
		else:
			head.next = self.deleteDuplicates(head.next)

		return head
```

---

## Similar Problems

- 

---

## Revisit

- [Solve again-3](Solve%20again-3.md)
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month