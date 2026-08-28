# 01 - Data Structures & Algorithms

Notes-first DSA learning: data structures, algorithmic patterns, core concepts, and a problem journal with code embedded inside the explanations.

## 01 - Data Structures

| Note | What it covers |
|------|----------------|
| [Array](01-Data%20Structures/Array.md) | O(1) index access, O(n) shifts |
| [Linked List](01-Data%20Structures/Linked%20List.md) | pointers, dummy head, reversal, cycles |
| [Doubly Linked List](01-Data%20Structures/Doubly%20Linked%20List.md) | prev + next, LRU, undo/redo |
| [Circular Linked List](01-Data%20Structures/Circular%20Linked%20List.md) | round-robin, traversal by identity |
| [Stack](01-Data%20Structures/Stack.md) | LIFO, matching/nesting |
| [Hash Table](01-Data%20Structures/Hash%20Table.md) | O(1) lookup, chaining, de-dup |

## 02 - Patterns

| Pattern | Reach for it when… |
|---------|--------------------|
| [Two Pointers](02-Patterns/Two%20Pointers.md) | pairs in sorted/ordered input |
| [Fast and Slow Pointers](02-Patterns/Fast%20and%20Slow%20Pointers.md) | linked-list middle, cycle |
| [Sliding Window](02-Patterns/Sliding%20Window.md) | contiguous subarrays/substrings |
| [Prefix Sum](02-Patterns/Prefix%20Sum.md) | repeated range-sum queries |
| [Divide and Conquer](02-Patterns/Divide%20and%20Conquer.md) | split → solve → combine |
| [Breadth-First Search](02-Patterns/Breadth-First%20Search%20%28BFS%29.md) | shortest path, level-by-level |

## 03 - Concepts

| Note | Idea |
|------|------|
| [Big-O Complexity](03-Concepts/Big-O%20Complexity.md) | the language of every trade-off |
| [Recursion](03-Concepts/Recursion.md) | base case + recursive case |
| [Binary Search](03-Concepts/Binary%20Search.md) | O(log n) on sorted data |
| [Sorting Algorithms](03-Concepts/Sorting%20Algorithms.md) | selection vs quicksort, Big-O |
| [Mutability vs Rebinding](03-Concepts/Mutability%20vs%20Rebinding.md) | Python reference semantics |

## 04 - Problem Journal

| # | Problem | Pattern(s) |
|---|---------|------------|
| | [Palindrome Linked List](04-Problems/234-palindrome-linked-list.md) | fast/slow + reverse |
| 83 | [Remove Duplicates from Sorted List](04-Problems/83-remove-duplicates-from-sorted-list.md) | traversal + pointer rewire |
| | [Merge Two Sorted Lists](04-Problems/merge-two-sorted-lists.md) | two pointers + dummy head |
| | [Reverse Linked List (Return Route Rebuild)](04-Problems/reverse-linked-list.md) | save–flip–advance |
| 1 | [Two Sum](04-Problems/0001-two-sum.md) | hash table |
| 217 | [Contains Duplicate](04-Problems/0217-contains-duplicate.md) | hash set |
| 1295 | [Find Numbers with Even Number of Digits](04-Problems/1295-find-numbers-with-even-number-of-digits.md) | string conversion |
| 20 | [Valid Parentheses](04-Problems/0020-valid-parentheses.md) | stack |
| 121 | [Best Time to Buy and Sell Stock](04-Problems/0121-best-time-to-buy-and-sell-stock.md) | sliding window / carry min |
| 1480 | [Running Sum of 1D Array](04-Problems/1480-running-sum-of-1d-array.md) | prefix sum |
| 705 | [Design HashSet](04-Problems/0705-design-hashset.md) | chained hash |
| 706 | [Design HashMap](04-Problems/0706-design-hashmap.md) | chained hash |

## How problems connect
Every problem links *up* to its pattern and data structure; every pattern/data-structure note links *down* to its problems. The graph is the point — in Obsidian open the Graph view; on GitHub use the tables above.