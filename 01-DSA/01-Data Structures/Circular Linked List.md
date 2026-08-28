# Circular Linked List

Over here the the last node `tail` points to the `head`. 

This is often used for Round robin tasks like *"which server gets a turn"*, *"which process gets a turn in the CPU"*, *"whose turn is it in a game"*.

Note: `while cur is not None` never terminates. we terminate when we arrive at where we started from, and we make use of `is` and not `==`, we check for the **object** and not the **value**. 

![Circular linked list](../../_attachments/Circular%20linked%20list%20e.g%2020260826225808.png)