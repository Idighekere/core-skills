---
number: "121"
title: 0121-best-time-to-buy-and-sell-stock
platform: leetcode
difficulty: easy
status: new
date: 2026-08-28
tags:
  - ds/array
---

# 0121-best-time-to-buy-and-sell-stock

**Link:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

## Summary
Describe the problem in your own words.

The goal is to buy at rhe least price and sell at the highest price tomake the most profit

## First thoughts
What was your initial idea?

I did struggle with this problem till i had to go watch tutorial.
I was thinking of using a nested loop to track the min price and the other future prices to buy from. I tried and tried and couldn't get it. Based on the example, I was thinking how I could get to pick 1, instead of 7, whcih was the first day. if i have to skip the first day, but this won't work for other test cases

## Key insight
What realization unlocked the solution?

## Solution
Explain the approach step by step.

The solution is to keep track of the minimum price(so we buy) and the max profit we can get.

1. Initialize two variables, `min_price` to be the first price, `max_profit` to be 0
2. iterate over the prices.
3. For every item, we check if the current price is less than the minimum price.
4. If it is, we set `min_price` to be the current price and do nothing if it ain't less than
5. We calculate the profit, if we sell it at that current pricem by subtracting `min_price` from current price
6. if the profit of the current price is greater than the max_profit we can make from previous trades, we update the max profit to the current profit.
7. At the end we return the max profit

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

```python
def max_profit(prices):
    min_price=prices[0]
    max_profit=0


    for price in prices:
        if price<min_price:
            min_price=price
        profit=price-min_price

        if profit > max_profit:
            max_profit=profit
    return max_profit




arr=[7,1,5,3,6,4]
arr2=[7,6,4,3,1]

print(max_profit(arr))
```

## Mistakes
What I got wrong the first time.

## Edge cases
Empty input, single item, duplicates, overflow, off-by-one.

## Learned
What transfers to other problems?

## Revisit
- [ ] Solve again in 3 days
- [ ] Solve again in 1 week
- [ ] Solve again in 1 month