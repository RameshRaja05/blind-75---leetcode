Sure! Here's your Java solution **fully documented in Markdown**, including the **problem statement**, **approach explanation**, and **code comments**.

---

## 🧩 Problem: Word Break (LeetCode 139)

> **Given** a string `s` and a list of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

You may assume that the dictionary does not contain duplicate words.

---

### 🔍 Example

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

---

## 🧠 Approach: Bottom-Up Dynamic Programming (Iterative)

- Use a boolean `dp[]` array where `dp[i]` means the substring `s[i:]` can be broken into words from the dictionary.
- Start from the end of the string and fill the `dp` array backwards.
- For each index `i`, iterate through every word in `wordDict`.
  - If the substring `s[i:i+word.length()]` matches a word and the remaining suffix is segmentable (`dp[i+word.length()]` is true), then mark `dp[i] = true`.

---

### ✅ Java Code

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int length = s.length();

        // dp[i] = true means s[i:] can be segmented into words in the dictionary
        boolean[] dp = new boolean[length + 1];
        dp[length] = true; // base case: empty string is always segmentable

        // Traverse the string from end to start
        for (int i = length - 1; i >= 0; i--) {
            // Try every word in the dictionary
            for (String word : wordDict) {
                int wordLen = word.length();

                // Check if the current word fits in the remaining string
                if ((i + wordLen) <= length && s.substring(i, i + wordLen).equals(word)) {
                    // If the word matches and the remaining substring is segmentable
                    dp[i] = dp[i + wordLen];
                }

                // If we've already confirmed dp[i] is true, no need to check further
                if (dp[i]) {
                    break;
                }
            }
        }

        // dp[0] tells us whether the full string can be segmented
        return dp[0];
    }
}
```

---

### 🧮 Complexity Analysis

| Type          | Complexity      |
|---------------|-----------------|
| Time          | O(n × m × k)    |
| Space         | O(n)            |

Where:
- `n` is the length of the string `s`
- `m` is the number of words in `wordDict`
- `k` is the average length of the words

---

Let me know if you'd like the same documentation for the **recursive or memoized** version too!