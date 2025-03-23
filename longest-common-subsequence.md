**Problem Statement:**
Given two strings `text1` and `text2`, return the length of their longest common subsequence (LCS). If there is no common subsequence, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (possibly none) deleted without changing the relative order of the remaining characters.

A **common subsequence** of two strings is a subsequence that appears in both strings.

---

**Example 1:**

Input: `text1 = "abcde"`, `text2 = "ace"`  
Output: `3`  
Explanation: The longest common subsequence is `"ace"`, and its length is `3`.

**Example 2:**

Input: `text1 = "abc"`, `text2 = "abc"`  
Output: `3`  
Explanation: The longest common subsequence is `"abc"`, and its length is `3`.

**Example 3:**

Input: `text1 = "abc"`, `text2 = "def"`  
Output: `0`  
Explanation: There is no common subsequence, so the result is `0`.

---

**Constraints:**
- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

---

## Approach:
The problem can be solved using **Dynamic Programming (DP)**.

### **1. Recursive Approach (Exponential Time Complexity - Not Efficient)**
- Define a recursive function that checks each character combination.
- If `text1[i] == text2[j]`, include it in LCS and recurse on `i+1, j+1`.
- Otherwise, explore both possibilities (`i+1, j` and `i, j+1`).
- **Time Complexity:** `O(2^n)` (Exponential - not practical for large inputs).

### **2. Dynamic Programming Approach (Efficient Solution)**
- Use a 2D DP table `dp[i][j]`, where `dp[i][j]` stores the LCS of `text1[0:i]` and `text2[0:j]`.
- Transition:
  - If `text1[i-1] == text2[j-1]`, then `dp[i][j] = 1 + dp[i-1][j-1]`.
  - Otherwise, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
- **Time Complexity:** `O(m * n)`, where `m` and `n` are lengths of `text1` and `text2`.
- **Space Complexity:** `O(m * n)` (can be optimized to `O(n)`).

---

## **Solution (Dynamic Programming - Bottom-Up Approach):**
```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = 1 + dp[i - 1][j - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    
    return dp[m][n]
```

---

## **Optimized Space Approach (1D DP Array):**
Instead of using a 2D table, we can optimize space to `O(n)` by using two 1D arrays:
```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    prev = [0] * (n + 1)
    
    for i in range(1, m + 1):
        curr = [0] * (n + 1)
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                curr[j] = 1 + prev[j - 1]
            else:
                curr[j] = max(prev[j], curr[j - 1])
        prev = curr
    
    return prev[n]
```

---

## **Conclusion:**
- **Recursive Approach**: Simple but inefficient (`O(2^n)`).
- **DP (Bottom-Up 2D Table)**: `O(m * n)` time and `O(m * n)` space.
- **Optimized DP (1D Array)**: `O(m * n)` time and `O(n)` space, making it the most efficient.

This problem follows the **classic DP pattern** and is used in applications like text comparison and DNA sequence analysis.

