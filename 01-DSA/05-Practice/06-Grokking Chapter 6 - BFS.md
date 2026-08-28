# Practice: Grokking Algorithms — Chapter 6 (Breadth-First Search)

Source: Grokking Algorithms book exercises (my own answers, kept verbatim).

## Exercise 6

## Run the breadth-first search algorithm on each of these graphs to find the solution.

### 6.1 Find the length of the shortest path from start to finish.

*(Exercise diagram — see book exercise 6.1, not included here.)*

2

### 6.2 Find the length of the shortest path from "cab" to "bat."

*(Exercise diagram — see book exercise 6.2, not included here.)*

2

## Reference code (mine)

```python
from collections import deque

"""
Assuming we have a problem where we want to find our FAceboook freind who is a mango seller.

Those directed connected to us is 1 degree
Those connected to those directly connected to use is 2 degree
and so on.

So we wanna find the shortest path to finding a mango seller.

To do this we have to check all our friends (1 degree) if we don't find, we check their friends and so on.

So we use a queue. when we check a friend and he is not. we add his friends to the end of the queue, remove him from the queue and continue withe next 1st degree friend in the queue.

We use hash table to demonstrate a graph in python.

graph={}

graph['me']=['bob','john','sam']
graph['bob']=['jane','jones']
graph['john']=[]
graph['sam']=['eric']
graph['jane']=[]
graph['jones']=[]
graph['eric']=[]


The array connected to me is my 1st degree connections
then those connected to bob.
john doesn't have any friend
sam has eric as a friend
jane and jones who are friends to bob has no friends
eric who is a friend to sam has no friends

and the double ended queue to demonstrate queues in python.

If a one person is a friend to more than one friend, you don't want to check the person twice or more. you just have to check once and add his friends to the queue.

If we check more than once. it will result into an infinite loop in some cases.

Assuming I have friends A,B and C and friend B has friend C,D and E.

If I check A. he has no firends and not a seller. I move to B.
I check B. he is not a seller. and i add his friends (C,D,E) to the queue.

The queue becomes [C,C,D,E].
I check C and he is not a seller and i add his friends. which is 'Me', this will result into starting again to check me.
"""

graph = {}

graph["me"] = ["bob", "john", "sam"]
graph["bob"] = ["jane", "jones"]
graph["john"] = []
graph["sam"] = ["eric"]
graph["jane"] = []
graph["jones"] = []
graph["eric"] = []


def search(name):
    """
    A simulated version of a bfs algorithm
    """
    search_queue = deque()  # create a new queue
    search_queue += graph[name]
    searched = set()  # to keep track of people i have searched.

    while search_queue:  # while the queue is not empty
        person = search_queue.popleft()  # grab the 1st person off the queue
        if not person in searched: #only search this person if you haven't searched before
            if person_is_seller(person):  # check whether the person sells mango
                print(f"{person} is a mango seller")  # yes he does
                return True
            else:
                search_queue += graph[person]
                searched.add(person) # mark person has searched
    return False


def person_is_seller(name):
    """
    A mock function to check if a person is mango seller
    """
    return name[-1] == "m"


search("me")
```