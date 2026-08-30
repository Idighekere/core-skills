---
number: ""
title: merge-two-sorted-lists
platform: synapse
difficulty: easy
status: new
date: 2026-08-22
tags:
  - ds/linked-list
  - pattern/two-pointers
---

# merge-two-sorted-lists

**Link:** https://synapseprep.dev/gym/interleaving-surge-queues

## Summary
Describe the problem in your own words.

There are two queue at a dispatch hub during surge pricing window,  so after the window we want to merge both queue and preserve the order in assending order. 

We must not convert the queues to list and sort them then merge. 


![Merging Two Driver Queues (excalidraw)](../../_attachments/Merging%20Two%20Driver%20Queues%202026-08-25%2013.45.27.excalidraw.svg)
## First thoughts 
What was your initial idea?
Since it involves arranging in a particular order and we shouldn't creatae a new node for to replace the actual node of the two list

Two pointers is a good fit for this. 
But i am thinking of first using the two pointer appraoch on both queues to sort them then go through the first queue.

Edit: I didn't notice that the queues are already sorted

hold a pointer to the head of the second queue, check the first queue the moment i find a node whose value is equal or greater than the head of the second queue, i rewire the pointer to invlude the second queue and then at the en of the second quee, i rewire it back to the frist queue.  

```python
from typing import Optional
# Platform type: ListNod
# - node.val is the node's value.
# - node.next is the next node, or None.

class Solution:

	def merge_driver_queues(self, hub_queue: Optional["ListNode"], overflow_queue: Optional["ListNode"]) -> Optional["ListNode"]:
	
		if hub_queue is None and overflow_queue is None:
			return None
		
		if hub_queue is None and overflow_queue:
			return overflow_queue
		
		if overflow_queue is None and hub_queue:
			return hub_queue
		
		curr1 = hub_queue
		curr2 = overflow_queue
		
		  
		
		while curr1 or curr2:
		
			temp_next1= curr1.next
			temp_next2 = curr2.next
			
			if curr1.val > curr2.val:
				curr2.next = curr1
				curr2 = temp_next2
			
			elif curr1.val <= curr2.val:
				curr1.next = curr2
				curr1 = temp_next1
		
		return curr1
```

## Key insight
What realization unlocked the solution?

After hours of struggles trying to make my solution work, i figured out that at some point the pointer of some node gets overwritten. 

So i went to youtube for help and I saw that the solution used dummy node. 
The dummy node approach will enable you to point tail which is `dummy.next` to the actaull value of the comparison (i.e the smallest node value)

## Solution
Explain the approach step by step.

1. Start by creating a dummy head which will act as a reference pointer., so we don't have to worry about empty state
2. Then get teh tail of the dummy node, this is where the smallest node value will be referenced from.
3. compare the first nodes of both queues
	1. if the list 1  node has a lower value we make the`tail.next` to point to list 1, then move over to the next item
	2. we do same for list 2, if list two has lower value, and then we move over to the next item in list 2, by doing `l2=l2.next`
4. after the comparison and reference pointing at the end we always want to move to the next tail node to avoid rewriting the pointers
5. also if a queue reaches the end, but the other doesn't we want to just move the remaining nodes of the other nodes to point to the tail since the nodes are sorted
6. At the end we gonna return `dummy.next`,  as dummy was just a reference point to our actual merged list while our merge listed started after the dummy node.


```python
from typing import Optional

# Platform type: ListNode
# - node.val is the node's value.
# - node.next is the next node, or None.

class Solution:

	def merge_driver_queues(self, hub_queue: Optional["ListNode"], overflow_queue: Optional["ListNode"]) -> Optional["ListNode"]:
	
		single_unified_driver_line= ListNode(0)
		cur = single_unified_driver_line
		
		while hub_queue and overflow_queue:
			
			if hub_queue.val >= overflow_queue.val:
				cur.next = overflow_queue
				overflow_queue = overflow_queue.next
			elif hub_queue.val < overflow_queue.val:
				cur.next = hub_queue
				hub_queue = hub_queue.next
			cur = cur.next

		if hub_queue:
			cur.next = hub_queue
		
		elif overflow_queue:
			cur.next = overflow_queue

		return single_unified_driver_line.next
```

The time complexity in the worst case depends on the length of the two queues, mostly the longest queue. 

- **Time Complexity:** O(m+n)
- **Space Complexity:** O(1)

## Mistakes
What I got wrong the first time.

-  I tried using the list reference to point to each other at somepoint the reference pointer will be lost. 
- 

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

-  Empty list: If both lists are empty, we return None
- If one is empty while the other ain't empty, we return the other.
- 

## Learned
What transfers to other problems?

 The problem forbids creating a new node to represent the actual nodes in the two queyes but never a reference pointer in this case the dummy node. 
The `tail/curr` node must not be `None` so that we don't run into an AttributeError when we try to access it's next

By using this approach we are just pointing the smallest node in the two queue to the tail of the current merged [Linked List](Linked%20List.md), 

## Revisit
- [Solve again-2](Solve%20again-2.md)
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month