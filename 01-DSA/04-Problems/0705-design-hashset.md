# [Design a Hashset]

- **Problem Link:** [Design HashSet](https://leetcode.com/problems/design-hashset/)
- **Difficulty:** [Easy]
- **Pattern:** [Hash Table]

## Thought Process

### Intuition

I thought of using linkedlists to store the keys after the hash functions runs, but i didn't really have an idea of how to create a Linkedlist node. SO i used help from google to create a linkedlist

### Approach

- I first create a new `ListNode` class to store the key and a pointer to the next node which is initially `None`
- For the hashset, I iniitalize the size. I use a prime number to reduce collision as they don't have factors except itself and 1.
- The buckets is just a list of Linked Lists nodes wtih a defaulted key to -1
- I create a hash function so I won't have to repeatedly write it. The hash function is just the modulo of the key with the size of the hashset.
- for the `add`.

1. We get the key and pass it througt the hash function to get an index.
2. We visit the index. by using the bracket notation on the buckets' list
3. We wanna ensure we don't add a key that already exist.
4. To check this we have to check every next node, till there's none (reach the last.). If we find a key that is same as ours, we do nothing. and keep checking.
5. IF we find no duplicate. we add the key to the end of the linked list.

- for the `remove`

1. We go through same initial step of hashing and locating the index
2. We need to check if we will find the key we want to remove in the next nodes. We do this until we reach the end. If we find it anywhere, we move the pointer of the next node after our immeditate next to point to our current node other wise we do nothing.

for the `contains`

1. We go through same initial step of ashing and location.
2. we check every node if there's a key that matches ours.
3. If there is, we return `True`, otherwise we keep checking till the end and then return `False`

### Complexity Analysis

- **Time Complexity:** O(1) on average, but O(n) on worst case. if the key isn't in the first node
- - `add`: O(1) on average, but O(n)
- **Space Complexity:** O(n) - We crete an extra data stricture, a list.

## Solution

```python
class ListNode:
    def __init__(self,key) -> None:
        self.key=key
        self.next=None

class MyHashSet:
    def __init__(self):
        self.size = 31 # we make use of a prime number to reduce collistion
        self.buckets=[ListNode(-1) for _ in range(self.size)]
    def _hash(self,key):
        return key%self.size
    def add(self,key)->None:
        index=self._hash(key)
        curr=self.buckets[index]


        while curr.next:
            if curr.next.key==key:
                return
            curr=curr.next

        curr.next= ListNode(key)


    def remove(self,key)->None:
        index=self._hash(key)
        curr=self.buckets[index]

        while curr.next:
            if curr.next.key==key:
                curr.next=curr.next.next
                return
            curr=curr.next

    def contains(self,key)->bool:

        index=self._hash(key)
        curr=self.buckets[index]

        while curr.next:
            if curr.next.key==key:
                return True
            curr=curr.next
        return False

myHashSet = MyHashSet();

print(myHashSet)
myHashSet.add(2)
myHashSet.add(8)
myHashSet.add(190)
myHashSet.remove(2)
print(myHashSet.contains(2))
print(myHashSet.contains(8))
print(myHashSet)
```