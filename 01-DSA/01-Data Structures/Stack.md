---
title: "Stack"
date: 2026-08-28
type: data-structure
tags: [ds/stack]
---

# Stack

## What it solves
LIFO ordering — the most recent thing goes out first. Built on top of a dynamic array; used anywhere you need "undo" semantics, nested matching, or an explicit recursion.

## Intuition
A stack of plates in a cafeteria. You take the plate from the top, and you put clean plates back on top. The plate that went in first is the last one you'll ever touch.

## Memory model
An array with a top pointer. Push = append at the end (O(1) amortized). Pop = remove from the end (O(1)). All the action happens at one end, so access to anything below the top is restricted.

## Operations

| Operation | Time | Why? |
|-----------|------|------|
| Push | O(1) | append at end (amortized) |
| Pop | O(1) | remove at end |
| Peek / Top | O(1) | read the end |
| Search for value | O(n) | must scan |

## Idioms & gotchas
- **The matching intuition:** for anything *nested* or *paired* (parentheses, tags, undo history), a stack works because the **last-opened thing must be the first one closed**.
- Map the closer to its opener to avoid index gymnastics:

```python
def is_valid_parenthesis(s):
    pairs = {")": "(", "]": "[", "}": "{"}
    stack = []
    for ch in s:
        if ch in pairs:                 # a closing bracket
            if not stack or stack[-1] != pairs[ch]:
                return False
            stack.pop()
        else:
            stack.append(ch)
    return len(stack) == 0
```

## Patterns it enables
- Validating/delimiters, monotonic stack (next greater element), explicit recursion (DFS).

Related problems:
- [Valid Parentheses](../04-Problems/0020-valid-parentheses.md)

## Variations
- **Monotonic stack** — maintains sorted order of values; the go-to for "next greater/smaller element".

## Pitfalls
- Forgetting the empty-stack check before `stack[-1]`.
- Using a stack when a queue (FIFO) is what the problem's phrasing demands — they're opposite.