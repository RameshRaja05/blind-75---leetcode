**Problem Statement:**
Given a string `s`, find the length of the longest substring without repeating characters.

**Constraints:**
- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols, and spaces.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Note: The answer must be a substring, not a subsequence.
```

---

**Approach:**
To efficiently find the longest substring without repeating characters, we use the sliding window technique with a HashMap to track character positions.

**Algorithm:**
1. Use two pointers, `i` and `j`, to denote the start and end of the current substring.
2. Traverse the string using `j`:
   - If `s[j]` is already in the HashMap, update `i` to `max(i, charMap[s[j]] + 1)` to skip duplicate characters.
   - Store `s[j]` and its index in the HashMap.
   - Update `res` with the maximum length found so far (`j - i + 1`).
3. Return `res` as the length of the longest substring without repeating characters.

---

**Java Solution:**
```java
import java.util.HashMap;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character, Integer> charMap = new HashMap<>();
        int i = 0, j = 0, res = 0;
        
        while (j < s.length()) {
            if (charMap.containsKey(s.charAt(j))) {
                i = Math.max(charMap.get(s.charAt(j)) + 1, i);
            }
            charMap.put(s.charAt(j), j);
            res = Math.max(res, j - i + 1);
            j++;
        }
        return res;
    }
}
```

---

**Algorithm: Longest Substring Without Repeating Characters**

This algorithm efficiently finds the length of the longest substring within a given string that contains no repeating characters. It utilizes a sliding window approach and a set to manage unique characters.

**Initialization:**

1.  Initialize a left pointer `l` to 0.
2.  Initialize an empty set `charSet` to track the unique characters within the current window.
3.  Initialize a variable `res` to 0, which will store the length of the longest substring found so far.

**Iteration:**

1.  Iterate through the string `s` using a right pointer `r`, from 0 to `len(s) - 1`.
2.  **Duplicate Check:**
    * While the character `s[r]` is already present in `charSet` (indicating a duplicate):
        * Remove the character `s[l]` from `charSet`.
        * Increment the left pointer `l` by 1 to shrink the window from the left.
3.  **Update Result:**
    * Calculate the current window size: `r - l + 1`.
    * Update `res` to be the maximum of its current value and the current window size: `res = max(res, r - l + 1)`.
4.  **Add Character:**
    * Add the character `s[r]` to the `charSet`.

**Return Result:**

1.  Return the final value of `res`, which represents the length of the longest substring without repeating characters.

**In essence:**

The algorithm employs a sliding window technique, where the window is defined by the left (`l`) and right (`r`) pointers. The `charSet` set ensures that only unique characters are within the window. The left pointer `l` expands the window when no duplicates are encountered and shrinks it when duplicates are found, ensuring that the window always contains a substring without repeating characters. The maximum window size encountered is tracked and returned as the result.


**Python Solution:**
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l = 0
        charSet = set()
        res = 0
        for r in range(len(s)):
            while s[r] in charSet:
                charSet.remove(s[l])
                l += 1
            res = max(res, r - l + 1)
            charSet.add(s[r])
        return res
```

**Time Complexity:**
- **O(n)**: Each character is processed once, making the solution linear.

**Space Complexity:**
- **O(min(n, m))**: The HashMap or Set stores up to `n` characters, where `m` is the character set size (e.g., 128 for ASCII).

This approach ensures optimal efficiency using the sliding window method.

