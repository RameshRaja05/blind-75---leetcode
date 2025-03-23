**Problem Statement:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

---

**Example 1:**

**Input:** prices = `[7,1,5,3,6,4]`  
**Output:** `5`  
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = `6 - 1 = 5`.  
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = `[7,6,4,3,1]`  
**Output:** `0`  
**Explanation:** In this case, no transactions are done and the max profit = `0`.

---

**Constraints:**
- `1 <= prices.length <= 10⁵`
- `0 <= prices[i] <= 10⁴`

---

**Approach:**

We need to find the maximum profit by keeping track of the **minimum price** encountered so far and computing the **maximum profit** at each step.

### **Optimized Approach: One-Pass (O(n) Time Complexity)**
1. Initialize `min_price` to a large value and `max_profit` to `0`.
2. Traverse through the `prices` array:
   - Update `min_price` whenever a new lower price is found.
   - Calculate profit (`prices[i] - min_price`) and update `max_profit` if it's higher than before.
3. Return `max_profit`.

**Time Complexity:** O(N)  
**Space Complexity:** O(1)  

**Implementation (Python):**

```python
def maxProfit(prices):
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        max_profit = max(max_profit, price - min_price)
    
    return max_profit
```

This approach ensures we make a single pass through the array, making it highly efficient for large inputs. 🚀

