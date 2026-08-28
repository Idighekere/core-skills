---
title: "Breadth-First Search (BFS)"
date: 2026-08-28
type: pattern
tags: [pattern/bfs]
---

# Breadth-First Search (BFS)

## Recognition clues
- **Shortest path** in an unweighted graph, or smallest number of steps.
- "Levels": visit everything 1 hop away, then 2 hops, etc.
- A problem whose data has *neighbours* (graph, tree, board cells, folder tree).

## When to use
When you need the **shortest path / fewest moves** (unweighted) or you explicitly want to explore level-by-level. BFS explores in waves — the first time you reach a node is guaranteed to be via the shortest route.

## Requirements
- A way to enumerate a node's neighbours.
- A **visited set** (or distance array) — otherwise cycles cause infinite loops.

## Generic algorithm
```python
from collections import deque

def bfs(start, goal):
    queue = deque([start])
    searched = set([start])

    while queue:
        node = queue.popleft()

        if is_goal(node):
            return True

        for neighbour in graph[node]:
            if neighbour not in searched:
                searched.add(neighbour)
                queue.append(neighbour)
    return False
```

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | O(V + E) — every vertex + edge once |
| Space | O(V) — queue + visited |

## Variations
- **Multi-source BFS** — seed the queue with many starts (e.g., all rotten oranges).
- **Level counter** — track distance per wave: drain the queue level-by-level (`for _ in range(len(queue))`).
- **DFS-shape on trees** — the same "print every file in a folder tree" logic, but with a stack instead of a queue:
```python
def print_names_bfs(start_dir):
    q = deque([start_dir])
    while q:
        d = q.popleft()
        for f in sorted(os.listdir(d)):
            p = os.path.join(d, f)
            print(f) if os.path.isfile(p) else q.append(p)
```

## Gotchas
- Forgetting the `visited` set → infinite loop on cyclic graphs.
- Queue vs stack for BFS vs DFS — swapping them changes traversal order completely.
- In trees there's only one ancestor path, so visited tracking is often implicit — but still needed if any cross-edge exists.

## Example problems
- Shortest path in an unweighted graph
- Number of islands (BFS/DFS over a grid)
- Word ladder

## Similar patterns
- **DFS** (stack/recursion) — the depth-first counterpart.
- [Divide and Conquer](Divide%20and%20Conquer.md) — recursive, but it *divides*; BFS *expands* level by level.