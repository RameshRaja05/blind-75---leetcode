# Longest Palindromic Substring

## Problem Statement

Given a string `s`, return the **longest palindromic substring** in `s`.

### Examples

**Example 1:**
- Input: `s = "babad"`
- Output: `"bab"`
- Explanation: `"aba"` is also a valid answer.

**Example 2:**
- Input: `s = "cbbd"`
- Output: `"bb"`

### Constraints

- `1 <= s.length <= 1000`
- `s` consists of only digits and English letters.

---

## Approach

To solve the problem of finding the longest palindromic substring, we use the **Expand Around Center** technique. This approach is both **efficient** and **intuitive**, and avoids the extra space used by dynamic programming solutions.

### Key Idea:

A palindrome reads the same forwards and backwards. This means it mirrors around its center.  
For any given palindrome, its center can be:

- A single character (like `"racecar"` → center at `'e'`)
- A pair of characters (like `"abba"` → center between `'b'`s)

Thus, for a string of length `n`, there are `2n - 1` possible centers:
- `n` odd-length centers (each character)
- `n - 1` even-length centers (between every two characters)

We **expand outward** from each of these centers to find the longest palindrome that can be formed from that center.

---

### Step-by-Step Walkthrough:

1. **Initialize** variables to keep track of the starting and ending indices of the longest palindrome found:  
   `start = 0`, `end = 0`

2. **Iterate** through each character in the string using an index `i`.

3. **Expand around center** in two ways for each index:
   - `len1`: Consider `i` as the center of an **odd-length** palindrome.
   - `len2`: Consider the gap between `i` and `i+1` as the center of an **even-length** palindrome.

4. **Calculate the maximum length** from the two expansions:
   ```java
   int len = Math.max(len1, len2);
   ```

5. **Update the `start` and `end` indices** if a longer palindrome is found:
   ```java
   if (len > end - start) {
       start = i - (len - 1) / 2;
       end = i + len / 2;
   }
   ```

   - These formulas correctly place the new substring bounds regardless of whether the palindrome length is odd or even.

6. **Return the substring** from `start` to `end` (inclusive) as the result:
   ```java
   return s.substring(start, end + 1);
   ```

---

### Helper Function: `expandAroundCenter`

This function does the core work:
```java
private int expandAroundCenter(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

It expands outward as long as:
- `left` and `right` are within bounds
- Characters at `left` and `right` match

The final length of the palindrome is `right - left - 1`, because after the last match, both pointers overstep.

---

### Time and Space Complexity

- **Time Complexity:** `O(n^2)`  
  We expand around each of `2n - 1` centers, and each expansion can go up to `O(n)` in the worst case.

- **Space Complexity:** `O(1)`  
  We only use constant extra space for variables; no additional data structures are needed.

---

## Code

```java
class Solution {
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }

    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";

        int start = 0;
        int end = 0;

        for (int i = 0; i < s.length(); i++) {
            int len1 = expandAroundCenter(s, i, i);       // Odd-length
            int len2 = expandAroundCenter(s, i, i + 1);   // Even-length
            int len = Math.max(len1, len2);

            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }

        return s.substring(start, end + 1);
    }
}
```

