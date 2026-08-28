---
title: "Mutability vs Rebinding"
date: 2026-08-28
type: concept
tags: [concept/python-memory]
---

# Mutability vs Rebinding

## Definition
Two completely different operations that look nearly identical in Python:

- **Mutate**: change the *object* a variable points to (add to it, modify its field).
- **Rebind**: make the variable point to a *different* object.

The single most repeated source of linked-list (and general reference) bugs.

## The rule
A variable is a **label on an object**, not a box storing it. So:

```python
x = Node("A")
y = x          # y labels the SAME node
y.data = "B"   # mutating the object → x sees "B"
```
vs
```python
x = Node("A")
y = x
y = Node("B")  # rebinding y → x is untouched, still "A"
```

> `y.data = "B"` **modified the object**. `y = Node("B")` **repointed the variable**.

## Why it wrecks linked-list code
```python
# ❌ "moving" by reassigning the walking variable never changes the list
cur = self.head
while cur is not None:
    cur = cur.next          # cur is a label; the list is untouched

# ✅ mutating the next field changes the structure
prev.next = cur.next        # real edit
```
**You modify a linked list by writing to a `.next` field — never by reassigning your loop variable.**

## Detection checklist
- "Am I assigning `=` (rebinding) or writing through a reference like `.next =` (mutating)?"
- "Do I still hold a handle to the original data, or did I rebind the only variable pointing to it?"
- Aliasing: `y = x` doesn't copy. If you need independent data, copy explicitly.

## Related
- [Linked List](../01-Data%20Structures/Linked%20List.md) — where this bites hardest.