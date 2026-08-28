# Practice: Grokking Algorithms — Chapter 2 (Selection Sort)

Source: Grokking Algorithms book exercises (my own answers, kept verbatim).

## Exercise 2

### 2.1 Suppose you're building an app to keep track of your finances. Every day, you write down everything you spent money on. At the end of the month, you review your expenses and sum up how much you spent. So you have lots of inserts and a few reads. Should you use an array or a list?

I will make use of linkedlist since I do much of inserts than reading and inserting an item to a linked list is much faster than to an array

### 2.2 Suppose you're building an app for restaurants to take customer orders. Your app needs to store a list of orders. Servers keep adding orders to this list, and chefs take orders off the list and make them. It's an order queue: servers add orders to the back of the queue, and the chef takes the first order off the queue and cooks it. Would you use an array or a linked list to implement this queue? **_(Hint: Linked lists are good for inserts/deletes, and arrays are good for random access. Which one are you going to be doing here?)_**

I will make use of linked list, since teh orers are taken in a sequential order that it was added and not randomly. And with linkedlist, the server can add an orde to the queue faster while the chef **removes** faster

### 2.3 Let's run a thought experiment. Suppose Facebook keeps a list of usernames. When someone tries to log in to Facebook, a search is done for their username. If their name is in the list of usernames, they can log in. People log in to Facebook pretty often, so there are a lot of searches through this list of usernames. Suppose Facebook uses binary search to search the list. Binary search needs random access—you need to be able to get to the middle of the list of usernames instantly. Knowing this, would you implement the list as an array or a linked list?

I will implement as an array, since it requires a randomised access and taht is much faster and possible using arrrays

### 2.4 People sign up for Facebook pretty often, too. Suppose you decided to use an array to store the list of users. What are the downsides of an array for inserts? In particular, suppose you're using binary search to search for logins. What happens when you add new users to an array?

Binary search requires a sorted array. So supposing a new user with name "Abba" is added, it can be added to the end, we have to shift all the billions of users in order to insert "Abba" at it's rightful position

### 2.5 In reality, Facebook uses neither an array nor a linked list to store user nformation. Let's consider a hybrid data structure: an array of linked ists. You have an array with 26 slots. Each slot points to a linked list. or example, the first slot in the array points to a linked list containing ll the usernames starting with A. The second slot points to a linked list ontaining all the usernames starting with B, and so on. uppose Adit B signs up for Facebook, and you want to add them o the list. You go to slot 1 in the array, go to the linked list for slot , and add Adit B at the end. Now, suppose you want to search for akhir H. You go to slot 26, which points to a linked list of all the Z ames. Then you search through that list to find Zakhir H. ompare this hybrid data structure to arrays and linked lists. Is it slower r faster than each for searching and inserting? You don't have to give big run times, just whether the data structure would be faster or slower.

**1. Searching**

- **Vs. Array: Slower.** In a sorted array, you use Binary Search ($O(\log n)$). In this hybrid, once you jump to the 'Z' slot, you are stuck doing a Linear Search ($O(n)$) through all the 'Z' names. Even though the list is smaller than the whole database, $O(n)$ is mathematically slower than $O(\log n)$ as $n$ grows.

- **Vs. Linked List: Faster.** In a pure Linked List, to find a 'Z' name, you have to walk through all the A, B, C... names first. Here, you skip straight to 'Z'.

**2. Inserting**

- **Vs. Array: Faster.** You don't have to "shift" millions of users to make room. You just find the right bin and drop the name in.

- **Vs. Linked List: Same.** Both allow for $O(1)$ insertion once you are at the correct spot.

## Reference code (mine)

```python
def find_smallest(arr: list) -> int:
    """
    :param arr: An array of integers in question
    :return int: the index of the smallest element in the array
    """

    smallest = arr[0]
    smallest_index = 0

    for i in range(1, len(arr)):
        if arr[i] < smallest:
            smallest = arr[i]
            smallest_index = i

    return smallest_index


my_arr = [1, 5, 5, 8, 2, 0]
print(find_smallest(my_arr))

"""
Considering 1,5,5,8,2,0

Initially smallest=1, smallest_index=0

for the for loop
when i=1
we compare if 5<1, and it's false

when i=2
same result

i=3
we compare if 8<1 and it's false, so smallest and smallest_index remains 1 and 0 respectively

i=4
we comapre if 2<1 and it's fasle

i=5
we compare if 0<1 and it's true
so we set smallest=0 and smallest index=5
then return 0
"""


def selection_sort(arr: list) -> list:
    """
    :param arr - list of items to sort
    :return list - sorted lists
    """
    new_arr = []
    copied_arr = list(arr)  # copy array before mutating

    for _ in  range(len(copied_arr)):
        smallest = find_smallest(copied_arr) # returns index of smallest value
        smallest_value=copied_arr.pop(smallest) #returns the value at the popped index
        new_arr.append(smallest_value)

    return new_arr

print(selection_sort(my_arr))
```