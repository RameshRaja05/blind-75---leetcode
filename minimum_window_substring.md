# LeetCode - Minimum Window Substring

## Problem Statement

Given two strings `s` and `t` of lengths `m` and `n` respectively, return the **minimum window substring** of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The test cases will be generated such that the answer is **unique**.

**Example:**
```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

**Constraints:**
- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 10⁵`
- `s` and `t` consist of uppercase and lowercase English letters.

---

## Algorithm Explanation

### ✅ Key Concepts:
- Sliding Window
- HashMap / Frequency Counter

### 🧠 Strategy:
1. **Need Map**: Count frequency of each character in `t`.
2. **Window Map**: Track characters in the current sliding window of `s`.
3. Expand the `right` pointer to find a valid window.
4. When all required characters are included (window is valid), move the `left` pointer to try shrinking the window while keeping it valid.
5. Track the smallest valid window found.

---

## 🪜 Step-by-Step Walkthrough

Given:
```text
s = "ADOBECODEBANC"
t = "ABC"
```

1. Build a frequency map for `t`:  
   `countT = {A:1, B:1, C:1}`

2. Start sliding window with two pointers `left` and `right` at 0.

3. Expand `right`, include characters into `window` and check if the current window contains all required characters.

4. Once all characters from `t` are in the window with required counts:
   - Try shrinking the window by moving `left` forward.
   - Update the result if this window is smaller than previous ones.

---

## 🧮 Code (Java)

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()) return "";

        Map<Character, Integer> countT = new HashMap<>();
        for (char c : t.toCharArray()) {
            countT.put(c, countT.getOrDefault(c, 0) + 1);
        }

        Map<Character, Integer> window = new HashMap<>();
        int have = 0, need = countT.size();
        int left = 0;
        int minLen = Integer.MAX_VALUE;
        int start = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            window.put(c, window.getOrDefault(c, 0) + 1);

            if (countT.containsKey(c) && window.get(c).equals(countT.get(c))) {
                have++;
            }

            while (have == need) {
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }

                char l = s.charAt(left);
                window.put(l, window.get(l) - 1);
                if (countT.containsKey(l) && window.get(l) < countT.get(l)) {
                    have--;
                }
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
    }
}
```

---

## 🔍 Time and Space Complexity

- **Time Complexity:** `O(s + t)`  
  - We traverse `s` once and build the `t` map once.
- **Space Complexity:** `O(s + t)`  
  - For maintaining frequency maps.

---

## 📊 Visual Example

```
s = "ADOBECODEBANC", t = "ABC"

Window expands:
[ADOBEC]ODEBANC --> contains A, B, C -> valid

Window shrinks:
A[DOBEC]ODEBANC --> move left to remove extra chars

Eventually:
ADOBECODE[BANC] --> minimum window that satisfies condition
```

---

## ✅ Summary

- Build frequency maps for characters in `t`.
- Use sliding window to expand and contract `s`.
- Track minimum valid window.
- This is an optimized solution with linear time complexity.

---

