# [Valid PArenthesis]

- **Problem Link:** [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
- **Difficulty:** [Easy]
- **Pattern:** [Stack]

## Thought Process

### Intuition

After hours of thinking of how to do it, i found out i only need to store the opening bracketsin a stack (we use stack, as a valid parenthesis will close the last openning bracket before the previous). and since the brackets are not same.

I thought of using two arrays one for the opeing and other for the closing, in the same order. 

### Approach

1. I keep track of the opening brackets that hasn't been closed in an stack. 
2. when we loop through the input string
3. I check if it is an opening bracket
4. if it is, i addit to the stack.
5. if it is closing bracket. i check if the top of the stack matches the corresponding opening bracket of the current bracket. if it does. we remove the item from the top
To acheive this using the two arrays of bracket. i get the index of the closing bracket and use it to get the corresponding open bracket, since they were arranged in same order. The same type of opening and closing bracket will have same index.
6. if the check in step 5 is false, it means the openbracket is not the same type as the clsing, therefore invalid parentheses
7. then it is neither a closing or opening bracket, then it's not a valid paretheses.
8. At the end of the day, the paretheses should be valid if the stack is empty. 

### Complexity Analysis

- **Time Complexity:** O(n) because we go through every single character once
- **Space Complexity:** O(n)

## Solution

```python
def is_valid_parenthesis(s):

    opening=['(','[','{']
    closing=[')',']','}']

    # I think I found out a way to use a dictionary to organize the brackets. using the closing as key and opening as value

    stack=[]

    for char in s:
        if char in opening:
            stack.append(char)
        elif stack and char in closing:
            index_of_closing=closing.index(char) #The opening and closing array must be arranged accordingly for this to work correctly
            if stack[-1]==opening[index_of_closing]:
                stack.pop()
            else:
                return False
        else:
            return False
    return len(stack)==0

print(is_valid_parenthesis("(){}[]"))
print(is_valid_parenthesis("([)"))
print(is_valid_parenthesis("()"))
print(is_valid_parenthesis(")()"))
```