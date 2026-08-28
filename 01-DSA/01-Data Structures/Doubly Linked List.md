# Doubly Linked List

This is a linkedlist but stores both a pointer to the `prev` and `next` node
![Doubly linked list](../../_attachments/Doubly%20linked%20list%2020260826222232.png)

They are useful in browser history, undo/redo, LRU cache eviction, music player prev/next. 
## Intuition
Explain it like I'm teaching a teenager.

Imagine you are using your browser and then you have visited some webpages. On the broswer you will see an arrow pointing to the right and another to the left. This is used to go forward to the next page you had visited, and the left arrow to go the the previous page

The forward is like `curr.next` and backward arrow is like `curr.prev`, this is just the simplest representatin of a doubly linked list.

### See Also: [Linked List](Linked%20List.md)