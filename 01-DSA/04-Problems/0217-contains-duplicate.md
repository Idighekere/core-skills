# Contains Duplicate

- **Problem Link:** [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
- **Difficulty:** Easy
- **Pattern:** Hash Set

## Thought Process

### Intuition
To find if any value appears at least twice, we need a way to "remember" the numbers we've already seen. A Hash Set is perfect for this because it allows for O(1) average time complexity for both insertions and lookups.

### Approach
1. Initialize an empty set.
2. Iterate through each number in the array.
3. If the number is already in the set, we found a duplicate; return `True`.
4. Otherwise, add the number to the set.
5. If we finish the loop without finding a duplicate, return `False`.

### Complexity Analysis
- **Time Complexity:** O(n) - We traverse the array once.
- **Space Complexity:** O(n) - In the worst case (no duplicates), we store all $n$ elements in the set.

## Solution

```python
def check_duplicate(nums):

    hash=set()

    for num in nums:
        if num in hash:
            return True
        hash.add(num)
    return False


print(check_duplicate([1,2,3,1]))
```