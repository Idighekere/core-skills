# Practice: Grokking Algorithms — Chapter 7 (Trees: BFS & DFS)

Source: Grokking Algorithms book exercise (my own answer, kept verbatim).

## Reference code (mine)

```python
from collections import deque
from os import listdir
from os.path import isfile, join


def print_names_bfs(start_dir: str):
    """
    A function that goes through every folder and prints its filename using BFS
    :param start_dir
    """

    search_queue = deque()  # we use a queue to keep track of the folders we will search
    search_queue.append(start_dir)

    while search_queue:
        dir = search_queue.popleft()

        for file in sorted(
            listdir(dir)
        ):  # loop through every file and folder in the dir
            full_path = join(dir, file)
            if isfile(full_path):
                print(file)
            else:
                search_queue.append(full_path)

def print_names_dfs(start_dir:str):
    """
    A function that goes through every folder and prints its filename using DFS
    :param start_dir
    """

    for file in sorted(listdir(start_dir)):
        full_path=join(start_dir,file)
        if isfile(full_path):
            print(file)
        else:
            print_names_dfs(full_path)

print_names_bfs("grokking-algorithms")
```