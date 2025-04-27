# LeetCode Problem: Decode Ways

## Problem Statement

(See the [problem description here](https://leetcode.com/problems/decode-ways/)).

You are given a string containing digits. Your task is to count how many different ways the string can be decoded based on the mapping:

```
'1' -> A, '2' -> B, ..., '26' -> Z
```

### Constraints:
- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zeros.

---

## Example Test Cases

### Example 1
**Input:** `s = "12"`  
**Output:** `2`  
**Explanation:** "AB" (1 2), "L" (12)

### Example 2
**Input:** `s = "226"`  
**Output:** `3`  
**Explanation:** "BZ" (2 26), "VF" (22 6), "BBF" (2 2 6)

### Example 3
**Input:** `s = "06"`  
**Output:** `0`  
**Explanation:** Starts with '0', which is invalid.

---

## Approach: DFS + Memoization (Top-Down DP)

### Idea

We recursively explore all valid decoding paths by:
- Taking 1 digit (if it's not '0')
- Taking 2 digits (if it's between 10 and 26)

We **memoize** the result of each index to avoid recomputation.

---

## Java Code

```java
class Solution {
    public int dfs(String s, int index, Map<Integer, Integer> dp, int n) {
        // Return memoized result if already computed
        if (dp.containsKey(index)) {
            return dp.get(index);
        }

        // '0' can't be decoded
        if (s.charAt(index) == '0') {
            return 0;
        }

        // Try taking 1 digit
        int res = dfs(s, index + 1, dp, n);

        // Try taking 2 digits if valid
        if (index + 1 < n &&
            (s.charAt(index) == '1' ||
             (s.charAt(index) == '2' && "0123456".indexOf(s.charAt(index + 1)) != -1))) {
            res += dfs(s, index + 2, dp, n);
        }

        // Save result
        dp.put(index, res);
        return res;
    }

    public int numDecodings(String s) {
        int n = s.length();
        Map<Integer, Integer> dp = new HashMap<>();
        // Base case: empty string can be decoded 1 way
        dp.put(n, 1);
        return dfs(s, 0, dp, n);
    }
}
```

---

## Example Walkthrough: `"11106"`

- `s = "11106"`
- Possible decoding paths:
  1. `"1 1 10 6"` → `"A A J F"`
  2. `"11 10 6"` → `"K J F"`

### Memoization Table Build-Up

```
index:  0   1   2   3   4   5
char:   1   1   1   0   6
dp:          ...filled via recursion...
dp[5] = 1    // empty string
dp[4] = 1    // '6'
dp[3] = 0    // '0' is invalid
dp[2] = 1    // '10'
dp[1] = 2    // '1 10', '11 0'
dp[0] = 2    // '1 1 10', '11 10'
```

✅ Final Output = 2

---

## Time & Space Complexity

- **Time Complexity:** `O(n)` — Each index is visited once due to memoization.
- **Space Complexity:** `O(n)` — Recursion stack + memoization map.

---

## Final Thoughts

This recursive method with memoization is elegant and intuitive. It avoids unnecessary recomputation and closely models how decoding decisions are made at each step. An iterative version using bottom-up DP is also an option for space optimization.


---

## Approach: Bottom-Up DP (Space Optimized)

### Idea

Instead of using a full DP array, this approach only keeps track of the **last two computed results**, as only `dp[i+1]` and `dp[i+2]` are needed to compute `dp[i]`.

---

## Java Code

```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int dp = 0;
        int dp1 = 1; // dp[n]
        int dp2 = 0; // dp[n+1]

        for (int i = n - 1; i >= 0; i--) {
            if (s.charAt(i) != '0') {
                dp += dp1; // valid single digit
            }
            if (i + 1 < n &&
                (s.charAt(i) == '1' ||
                (s.charAt(i) == '2' && "0123456".indexOf(s.charAt(i + 1)) != -1))) {
                dp += dp2; // valid two-digit number
            }

            dp2 = dp1;
            dp1 = dp;
            dp = 0;
        }

        return dp1;
    }
}
```

---

## Example Walkthrough: `"11106"`

We work from right to left.

### State Updates:

```
Initial: dp1 = 1, dp2 = 0

i = 4 ('6'):  dp = 1      → dp1 = 1, dp2 = 1
i = 3 ('0'):  dp = 0      → dp1 = 0, dp2 = 1
i = 2 ('1'):  dp = 1      → dp1 = 1, dp2 = 0
i = 1 ('1'):  dp = 2      → dp1 = 2, dp2 = 1
i = 0 ('1'):  dp = 2      → dp1 = 2, dp2 = 2
```

**Valid Decodings:**
- `'1 1 10 6'` → `'A A J F'`
- `'11 10 6'` → `'K J F'`

✅ Final Result: **2**

---

## Time & Space Complexity

- **Time Complexity:** `O(n)` — single loop over the string
- **Space Complexity:** `O(1)` — only three variables used

---

## Final Thoughts

This version is compact and efficient. It’s ideal when memory is limited and performance is key. It’s also less prone to stack overflow than the recursive version on large inputs.
