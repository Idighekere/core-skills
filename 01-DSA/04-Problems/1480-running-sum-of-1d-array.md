# [Running Sum of 1d array]

- **Problem Link:** [Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/)
- **Difficulty:** [Easy]
- **Pattern:** [Array]

## Thought Process

### Intuition

When i initially saw the problem, i thought of using recursion, seting the base case to return the array if empty or length is one, but that didn't, then i thought of using nested loops. storing a total somewherea and then using adding the current number of the inner loop to what so ever toatl was in the outer loop.

The nested loop was just an overthinking

### Approach

1. I initiallized an empty array and a `total` variable to 0.
2. Loop throught the array once, for every element.
3. I add the current element to the `total` and store the result in `total`
4. Then append the update `total` to the result array
5. Then the array is returned

### Complexity Analysis

- **Time Complexity:** O(n) as i loop through the array once
- **Space Complexity:** O(n) I created a new array to store the result,adding every element to it.

## Solution

```python
def running_sum(nums):

    summ=0
    result=[]

    for num in nums:
        summ+=num
        result.append(summ)

    return result



arr=[1,2,3,4]
arr2=[1,1,1,1]
print(running_sum(arr))
print(running_sum(arr2))
```