---

# 🪙 Coin Change – Dynamic Programming

## 📝 Problem

You are given:
- An array `coins[]` of coin denominations
- An integer `amount` for the total money needed

🔁 Return the **fewest number of coins** needed to make up that amount. If it's not possible, return **-1**.

You have **infinite supply** of each coin.

---

## 🧠 Approach: Bottom-Up Dynamic Programming

### ✅ Step-by-Step Strategy

1. Initialize a `dp[]` array of size `amount + 1`, filled with `amount + 1` (impossible max value).
2. Set `dp[0] = 0` — it takes 0 coins to make amount 0.
3. For each coin and every sub-amount `a` from coin to amount:
   - `dp[a] = min(dp[a], 1 + dp[a - coin])`
4. Return `dp[amount]` if it’s updated, else return `-1`.

---

## 💻 Java Implementation

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int max = amount + 1;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, max);
        dp[0] = 0;

        for (int coin : coins) {
            for (int a = coin; a <= amount; a++) {
                dp[a] = Math.min(dp[a], dp[a - coin] + 1);
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

## 📊 Complexity

| Metric | Value        |
|--------|--------------|
| Time   | O(amount * n) |
| Space  | O(amount)     |

Where `n` = number of coin types.

---

## 🧪 Examples

### Example 1:
```text
Input: coins = [1, 2, 5], amount = 11
Output: 3 → 5 + 5 + 1
```

### Example 2:
```text
Input: coins = [2], amount = 3
Output: -1 → Can't make 3 using only 2s
```

### Example 3:
```text
Input: coins = [1], amount = 0
Output: 0 → No coins needed
```

---

## 📌 Visual Trace

Let’s trace `coins = [1, 2, 5]`, `amount = 11`

```text
dp = [0, ∞, ∞, ∞, ∞, ∞, ∞, ∞, ∞, ∞, ∞, ∞]

After using 1:
dp = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

After using 2:
dp = [0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6]

After using 5:
dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3] ✅
```

Result: `dp[11] = 3`

---

Would you like to add a recursive + memoized top-down version too?

