---
number: ""
title: reverse-linked-list
platform: synapse
difficulty: easy
status: new
date: 2026-08-22
tags:
  - ds/linked-list
---

# reverse-linked-list

**Link:** https://synapseprep.dev/gym/return-route-rebuild

## Summary
Describe the problem in your own words.

A food delivery driver does stop at some spots so we are to retrace the routes by going starting from his last stop to the previous until where it started.

So this is basically a linked list reversal problem the first stop links to the next and so on 

1 -> 2 -> 3 ->


```python
order = [14, 27, 33, 30] => [30, 33, 27, 14]
order = [] => None
order = [5] = [5]
```

## First thoughts
What was your initial idea?
Tbvh, I thought of ways to handle the unlinking, by just swapping a nodes previous with the next and the next with a prev, but with this I won't be able to move to the next node because i have set it to `None` already

## Key insight
What realization unlocked the solution?

- I needed a temperorary storage to keep `curr.next` value while i set `curr.next = prev` for me to be able to still use `curr.next` whithout loosing it's initial pointer

## Solution
Explain the approach step by step.
Looking at the test cases, an empty list should return `None` and a single item list should return that item

1. Iterate over all the list item including the last item which means `curr.next` won't be in the while loop statement
2. store the `curr.next` value to a `temp` variable
3. set `curr.next` to `prev`
4. then set `prev` to `curr`
5. we can then still have access to the initial `curr.next` which is now  `temp` to set `curr` to `temp` for a next iteration
6. at the end, we return `prev`, because `prev` will hold the value of the last iteration `curr` value

```python

class Solution:

	def rebuild_return_route(self, order: Optional["ListNode"]) -> Optional["ListNode"]:

		curr = order
		
		if curr is None:
			return None
		
		if curr.val and curr.next is None:
			return curr
		
		prev = None
		
		while curr:
			# curr.next, prev = prev, curr.next
			temp_next = curr.next
			curr.next = prev			
			prev = curr			
			curr = temp_next
		
		return prev
```

We have to iterate over every single item in the list and perform a O(1) task rewiring pointers `curr.next` -> `prev`, `prev` -> `curr` and `curr` -> `curr.next`
Making it a time complexity of O(n)

but by not introducing an extra space in the iterative approach

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

- Nill for now

## Mistakes
What I got wrong the first time.

-  I did `if not curr.value`, which checks for the truthy of the value and not if it's  a none empty node
	- by doing the previous, it also breaks if the node is empty, because i dirrectly accessed `curr.val`
- I also did `while curr and curr.next is not None`, the second clause made the loop to stop without working on the last item of the list because `curr.next = None`

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

-  **empty list**: handled by the early if check, without the if check it is still hanled because, `prev = None` initially and then the loop won't run as `curr = None` and then it returns `prev` which is `None`
- **single item:** the second if check also works, the edge case is still handle regardless of the if check, as `prev` and `curr.next` are both None, and `curr ` and `prev` are both the only available node.

## Learned
What transfers to other problems?

-  Always be mindfull of the if and while conditions, a wrongly written statement might skip nodes, throw key error

## Revisit
- [Solve again](Solve%20again.md)
- [ ] Solve again in 1 week 
- [ ] Solve again in 1 month