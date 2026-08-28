# Data Structure: LinkedList

Date: 2026-08-13, 01:40
## Prerequisites

- [Array](Array.md)

## Overview
A Linkedlist is a datastructure where elements are connected to another by references in sequence, 

Each element is called a `Node`

The **head** does not store the value of the first node but a reference to the first node. It is not a node

![Example Linkedlist](../../_attachments/Example%20Linkedlist.png)

An empty list has `head = None`
The last node (**tail**) has `next = None`

## Internal Structure
**It doesn't really mean that the data is stored continuously in memory**

> [!NOTE] When reordering an element in a linkedlist, we must always **establish the new reference before destroying the old**
> From the image, if we try to move `Node D` to be the first element. 
> We must first set `Node_D.next = Node_A`, then `head = Node_D`.
> If we do the second before first, we are disregarding `Node_A` to `Node_D`, because the `Node_D.next = None`, so setting it it to be `head` makes `Node_D` the first and last node.

#### Mutating an object vs rebinding a variable.
When we write `x = Node('A')`, it gets stored in a memory and `x` holds a reference to the **address** and not the ~~**object**~~ itself

```python
x = Node("A") 
y = x # y refers to the SAME node, not a copy 
y.data = "B" 
print(x.data) # "B" ← x changed too, because there is only one node 
```

```python
x = Node("A") 
y = x 
y = Node("B") # this REPOINTS y at a different object 
print(x.data) # "A" ← x is untouched 
```


> [!INFO] `y.data` = "A" modified the object whereas `y = Node("B")` repointed the variable

## Core Operations

| Operation | Time | Why?                                                                              |
| --------- | ---- | --------------------------------------------------------------------------------- |
| Access    | O(n) | You can only access data sequential, they don't make use of indexes               |
| Search    | O(n) | You can't search since there are no indexes, `n` is the number of nodes available |
| Insert    | O(1) | There's no physical reloaction, just update the pointer                           |
| Delete    |      |                                                                                   |
| Update    | O(1) |                                                                                   |

> **Almost every linked list bug is a correct set of operations performed in the wrong order.**

### 1.  Traversal, and two common errors
This operation is **O(n)**

```python
# ✅
def traverse(self):    
	# a separate walker    
	cur = self.head    
	while cur is not None:        
		play(cur.data)        
		cur = cur.next
		
		
# ❌ - The loop will exit without running the last node
def traverse(self):    
	cur = self.head   
	
	while cur.next is not None:  # ← asks about the NEXT node     
		play(cur.data)        
		cur = cur.next
		
# ❌ - by moving head, we destroy the previous nodes.
def traverse(self):    
	# no separate walker at all    
	while self.head is not None:        
		play(self.head.data)             
		self.head = self.head.next  # ← moves your only handle  
```

> [!INFO] **The idiom to memorise:**
> ```python
> # visit every node 
> while cur: 
> # I need to look one node ahead 
> while cur and cur.next:
> ```

### 2. Insertion at the head: two writes, and their order 
This operation is **O(1)** because no matter the length of the list or number of nodes. only two operations occurs.

```python
# ✅ - wire up your new connections before you tear down old ones.
def insert_at_head(self, data):
    new_node = Node(data)       
    new_node.next = self.head      # 1. point into the existing playlist 
    self.head = new_node # 2. now move head  
    
❌ 
def insert_at_head(self, data):    
	new_node = Node(data)      
	self.head = new_node    # 1. move head FIRST  
	new_node.next = self.head # 2. ...point at what, exactly?    
```

### 3. Insertion at the tail, and the empty-list case
This is **O(n)**, because you have to traverse from the first to the tail before you can add. To make it **O(1)**, we can track `self.tail` alongside `self.head`, but this come with cost because we need to update `self.tail` everytime we add or remove. 
- Trying to make time complexity O(1) means we aare trading if with more space.
- Removing the last time will alsways be **O(n)**, because we need the second-to-last element to become `self.tail`

```python

def insert_at_tail(self, data):    
	new_node = Node(data)     
	# empty playlist — special case    
	if self.head is None:        
		self.head = new_node        
		return   
	cur = self.head    
	# stop AT the last song    
	while cur.next is not None:        
		cur = cur.next    
	cur.next = new_node
```

> [!INFO] 
> when you need to _read_ every node, loop `while cur.` When you need to _modify_ a node, loop `while cur.next` to stop while you still have a reference to it.

### 4. Insertion after a held node: the O(1) case
It's O(1) because we have a target node we want to insert it at, we aren't iterating over the entire list. 

```python
def insert_after(self, node, data):
	if node is None:
		raise ValueError("node must not be None")
		
	new_node = Node(data)
	
	new_node.next = node.next # 1. claim the rest of the playlist
	node.next = new_node # 2. then relink the predecessor
	
	
```

### 5. Deletion by value, and the need for a predecessor

To remove node X, you must edit the node **before** X.
We need to keep track of the `prev`, because as soon as `curr` moves to another node, there's no way for it to know the previous node in a singly linked list so it can it can link the previous to the next when the node X is found.

When deleting we need to handle **6** edge cases
1. middle item (the baseline): `prev.next = curr.next` handles it
2. empty list: `cur is not None` in the head check handles it
3. first item: the first `if` block hanles it
4. only one item: `self.head` becomes `None`
5. last item: `curr.next = None` so `prev.next = None` too at the unlining stage
6. Absent: `if curr is None` handles it, it checks after it has gone through all the nodes and the `curr` is none because the previous node had the next = None

```python
	def delete(self,data):
		curr = self.head
		
		#removing the first node
		if curr is not None and curr.data == data:
			self.head = curr.next #just make head to point to the next
			return True
		
		prev = None
		
		while curr is not None and curr.data != data:
			prev=curr
			curr=curr.next
		
		#goes throught all the nodes without locating the intended node
		if curr is None:
			return False
			
		#unlink	
		prev.next=curr.next
		return True
			
```

> [!INFO] **Checklist to consider when deleting**
> `empty`, `single`, `first`, `last`, `absent`

**You modify a linked list by writing to a .next field. Never by reassigning your loop variable.**


### Dummy head or Sentinel node
This is like a kind of fake front node.
With this we don't have to worry about checking if it's the first node in the list.

```python
					##### BEFORE ######
def delete(self, data): 
# ← head special case 
	if self.head and self.head.data == data: 
		self.head = self.head.next 
		return 
	prev, cur = None, self.head 
	
	while cur and cur.data != data: 
		prev, cur = cur, cur.next 
	
	if cur: 
		prev.next = cur.next
		
					###### AFTER #######

def delete(self, data):
	dummy = Node(0)
	dummy.next = self.head
	
	prev = dummy
	
	while prev.next and prev.next.data != data:
		prev = prev.next
	
	if prev.next:
		prev.next = prev.next.next

	#hanldles head-was-removed automatically
	self.head = dummy.next
```

With this approach, it correctly handles when the node to be delted is the fisrt, the only node and an empty

>[!INFO]   when a problem might require modifying teh `head`, start by using the dummy node `dummy = Node(0) dummy.next = head` and then `return dummy.next` at the end of the function


### Inplaced reversal of a linkedlist
The steps involved
1. **SAVE** the rest of the list
2. **FLIP** the arrow backward
3. **ADVANCE** the prev
4. **ADVANCE** the curr

```python
def reverse(head):
	prev = None
	cur = head
	
	while cur is not None:
		# SAVE
		nxt = cur.next
		# FLIP
		cur.next = prev
		#ADVANCE
		prev = cur
		#ADVANCE
		cur = nxt
	# cur is None; prev is the new first stop
	return prev
```

```python
def reverse_recursive(head):
	# base case: if we reach the last node or if the list is emptu
	if head is NOne or head.next is None:
		return head
	#Dive all the way to the end of the list	
	new_node = reverse_recursive(head.next)
	head.next.next = head # Make the NEXT node point back to ME
	head.next = None # I now terminate the list
	#Return the new head (the original tail node) all the way up
	return new_node
```

###  Locating the middle node in a single pass (Fast and Slow Pointer)
To locate the middle note without having to go throught the list **twice**, we make use of two pointers. **the fast and slow pointers**, the **slow** goes one step while the **fast** goes two steps, by the time the fast reaches the end of the list, the slow will be at half it's position which is the midpoint.

Without using this appraoch, we will have to go trhough the whole list once, then determine the lenght and calcualte where the middle node will be, and then go through it again for the second time. 

```python
def find_middle(head):
	slow = fast = head
	
	while fast is not None and fast.next is not None:
		slow = slow.next
		fast = fast.next.next
	return slow
```

This approach works for both even and odd number of nodes, but even-length lists have no single middle, so i must know which i want to return.
To get the **first** middle our condition will have to change to `while fast.next and fast.next.next`

### Cycle detection in constant space (Floyd's algorithm)

```python
def has_cycle(head):
	slow = fast = head
	
	while fast is not None and fast.next is not None:
		slow = slow.next
		fast = fast.next.next
		
		if slow is fast:
			return True
	return False
```

### Locating the kth node from the end
In the scenario of a browser history where the oldest visited page is stored first and the last is store at the end. 

In order to retrieve the **3rd most recent**, that is where this comes in. we are retrieving the 3rd from the end, and we need to do this without going through the entire list from the start.

This is helpfull in solving the delete nth node from the end problem

```python
def remove_nth_from_end(head, n):

	dummy = ListNode()
	dummy.next = head
	
	slow = fast = dummy
	
	# advance the fast pointer first to be n step ahead
	for _ in range(n):
		if fast.next is None:  
		 # history shorter than n            
		 return head
	
	# we will now advance both pointers, with fast leading n steps ahead and we stop the moment the fast pointer is at the last node
	while fast.next is not None:
		slow = slow.next
		fast = fast.next
		
	# as fast points to last, the slow the pointer will be at the node before the one we wanna remove, so we rewire
	slow.next = slow.next.next
	
	return dummy.next
```


## Five recurring patterns

| Pattern               | Reach for it when…                  | Seen here as                      | Classic problems                       |
| --------------------- | ----------------------------------- | --------------------------------- | -------------------------------------- |
| **Dummy head**        | The head might change or be removed | Unfollow an artist · nth from end | Remove elements, merge, partition      |
| **Fast/slow (2×)**    | Middles, cycles, halves             | Split a route · org chart cycle   | Find middle, detect cycle, palindrome  |
| **Offset pointers**   | "kth from the end"                  | Browser history                   | Remove nth from end, rotate            |
| **Save–flip–advance** | Reversing or reordering             | Drive the route home              | Reverse list, reverse sublist, reorder |
| **Trailing prev**     | Removing in a singly linked list    | Remove a song · dedupe readings   | Delete by value, remove duplicates     |

## Common Operations
- delete
- insert
- update


# Problems to practice
LeetCode, roughly in order: 876 (Middle) · 206 (Reverse) · 141 (Cycle) · 21 (Merge Two Sorted) · 203 (Remove Elements) · 83 (Remove Duplicates) · 19 (Remove Nth From End) · 234 (Palindrome) · 92 (Reverse Sublist) · 143 (Reorder List) · 142 (Cycle II) · 146 (LRU Cache — the payoff problem)

## Common Mistakes
Repeatedly trying to access `.next` of a node and then `data` of a  last node which the `next = None`