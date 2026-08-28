---
number: "706"
title: 0706-design-hashmap
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/hash-table
---

# 0706-design-hashmap

**Link:** https://leetcode.com/problems/design-hashmap/

## Summary
Describe the problem in your own words.

## First thoughts
What was your initial idea?

I thought of using linkedlists to store the keys after the hash functions runs, but i didn't really have an idea of how to create a Linkedlist node. SO i used help from google to create a linkedlist

## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

- I first create a new `ListNode` class to store the key and value and a pointer to the next node which is initially `None`
- For the hashset, I iniitalize the size. I use a prime number to reduce collision as they don't have factors except itself and 1.
- The buckets is just a list of Linked Lists nodes wtih a defaulted key to -1
- I create a hash function so I won't have to repeatedly write it. The hash function is just the modulo of the key with the size of the hashset.
- for the `put`.

1. We get the key and value and pass it througt the hash function to get an index.
2. We visit the index. by using the bracket notation on the buckets' list
3. We wanna ensure we don't add a key that already exist.
4. To check this we have to check every next node, till there's none (reach the last.). If we find a key that is same as ours, we do update the value. and keep checking.
5. IF we find no duplicate. we add the key with it's value to the end of the linked list.

- for the `remove`

1. We go through same initial step of hashing and locating the index
2. We need to check if we will find the key we want to remove in the next nodes. We do this until we reach the end. If we find it anywhere, we move the pointer of the next node after our immeditate next to point to our current node other wise we do nothing.

for the `get`

1. We go through same initial step of ashing and location.
2. we check every node if there's a key that matches ours.
3. If there is, we return the value, otherwise we keep checking till the end and then return -1

- **Time Complexity:** O(1) on average, but O(n) on worst case. if the key isn't in the first node
- - `put`: O(1) on average, but O(n)
- **Space Complexity:** O(n) - We crete an extra data stricture, a list.

```python
class ListNode:
    def __init__(self,key,value=None):
        self.key=key
        self.value=value
        self.next=None

class MyHashMap:
    def __init__(self):
        self.size=31
        self.buckets=[ListNode(-1) for _ in range(self.size)]

    def _hash(self,key):
        return key%self.size

    def put(self,key,value)->None:
        idx=self._hash(key)
        curr=self.buckets[idx]

        while curr.next:
            if curr.next.key==key:
                curr.next.value=value
                return
            curr=curr.next

        curr.next=ListNode(key,value)

    def get(self,key)->int|None:
        idx=self._hash(key)
        curr=self.buckets[idx]

        while curr.next:
            if curr.next.key==key:
                return curr.next.value
            curr=curr.next

        return None

    def remove(self,key)->None:
        idx=self._hash(key)
        curr=self.buckets[idx]

        while curr.next:
            if curr.next.key==key:
                curr.next=curr.next.next
                return
            curr=curr.next

myHashMap = MyHashMap();

print(myHashMap)
myHashMap.put(2,5)
myHashMap.put(8,4)
myHashMap.put(190,56)
myHashMap.remove(2)
print(myHashMap.get(2))
print(myHashMap.get(8))
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