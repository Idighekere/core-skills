---
title: "Hash Table"
date: 2026-08-28
type: data-structure
tags: [ds/hash-table]
---

# Hash Table

## What it solves
O(1) average **lookup, insert, and delete by key**. It converts a key (string, int, anything hashable) into an array index via a hash function, so you never scan for a value — you compute where it lives.

## Intuition
A library with a perfect catalog. You don't search shelf by shelf; you compute the aisle number from the book title, walk straight there, and grab it.

## Memory model
A **bucket array** + a **hash function**. `index = hash(key) % num_buckets`. Collisions (two keys, one bucket) are resolved by storing a chain — usually a [Linked List](Linked%20List.md). Python's `dict` and `set` are hash tables.

```python
# In Python, dicts are hash tables (mapping key -> value)
book = {}
book["apple"] = 0.67
book["avocado"] = 1.49
book["milk"] = 1.49
print(book)
```

### De-duplication with a set — the classic trick
```python
voted = {}
def check_voter(name):
    if name in voted:
        print("Kick them out — already voted")
    else:
        voted[name] = True
        print("Let them vote")
```

## Operations

| Operation | Average | Worst | Why |
|-----------|---------|-------|-----|
| Lookup / get | O(1) | O(n) | hash → bucket; worst = all keys collide |
| Insert / put | O(1) | O(n) | same |
| Delete | O(1) | O(n) | same |
| Iterate | O(n) | O(n) | must touch every bucket |

## Idioms & gotchas
- **"Complement / seen-so-far"** — the template that turns an O(n²) pair-scan into O(n):
```python
def two_sum(nums, target):
    seen = {}          # value -> index
    for i, num in enumerate(nums):
        comp = target - num
        if comp in seen:
            return [i, seen[comp]]
        seen[num] = i
```
- **Bucket size matters.** A prime number of buckets reduces collisions (no shared factors with common key patterns).
- A good hash function is **consistent**: the same input always hashes to the same output. `random()` as a hash = broken (you'd never find your item again).

## Patterns it enables
- Hashing (chaining) implementations, counting (frequency maps), de-duping, caches.

Related problems:
- [Two Sum](../04-Problems/0001-two-sum.md)
- [Contains Duplicate](../04-Problems/0217-contains-duplicate.md)
- [Design HashSet](../04-Problems/0705-design-hashset.md)
- [Design HashMap](../04-Problems/0706-design-hashmap.md)

## Variations
- **Chained hash table** — each bucket is a linked list (used extensively in the design-hashset/hashmap problems).
- **Direct-address table** — keys are already array indexes (no hashing needed).
- **Open addressing** — collisions probe for the next free slot instead of chaining.

## Pitfalls
- Average case is O(1), but a bad hash function or full table degrades to O(n).
- Forgetting the data structure entirely — any "have I seen this?" problem is a set/dict in disguise.