# Practice: Grokking Algorithms — Chapter 3 (Recursion)

Source: Grokking Algorithms book exercises (my own answers, kept verbatim).

## Exercise 3

### 3.1 Suppose I show you a call stack like this. What information can you give me, just based on this call stack?

*(Exercise diagram — see book exercise 3.1, not included here.)*

The Stack uses the LIFO principle, so the GREET2 function which entered last will be executed first and return control to the GREET function

### 3.2 Suppose you accidentally write a recursive function that runs forever. As you saw, your computer allocates memory on the stack for each function call. What happens to the stack when your recursive function runs forever?

The stack will get filled up with lots of function calls' variable and reach it's stack limit leading to a **Stack Overflow**

## Reference code (mine)

```python
def factorial(n):
    """
    :param n - the number to find the factoruial
    :return int -   the factoruial of n
    """

    # the base case
    if n==0:
        return 1
    return n*factorial(n-1)


print(factorial(5))

def fib(n):
    """
    :param n - the number to find the fibonacci
    :return list
    """

    # if n==0:
    #     return 0
    # if n==1:
    #     return n
    if n in (0,1):
        return n
    return fib(n-1)+fib(n-2)

print(fib(7))
```