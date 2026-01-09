# LeetCode 424: Longest Repeating Character Replacement

## Problem Statement

Given a string `s` and an integer `k`, you can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

### Example:

**Input:**

```java
s = "AABABBA", k = 1
```

**Output:**

```
4
```

**Explanation:**
Replace the one 'A' in the middle with 'B' to form "AABBBBA". The longest substring is now "ABBB" or "AABB" (length 4).

---

## Constraints

* `1 <= s.length <= 10^5`
* `s` consists of only uppercase English letters.
* `0 <= k <= s.length`

---

## Approach 1: Sliding Window (Optimized with Frequency Array)

This problem is solved using the **sliding window technique**:

1. Maintain a window `[left, right]` over the string.
2. Count the frequency of characters in the current window.
3. Keep track of the **maximum frequency** of any character in the current window.
4. If the number of characters that need to be replaced in the window is greater than `k`, shrink the window from the left.
5. Track the maximum valid window length seen so far.

### Algorithm Walkthrough

* Initialize a frequency array `freq[26]` to track character counts.
* Use two pointers: `left` and `right`.
* For each character `s[right]`, increment its frequency.
* Track the `maxFreq` character count in the window.
* Check if the window is valid: `(right - left + 1) - maxFreq <= k`.

  * If not valid, shrink the window from the left.
* Track the maximum valid window size `maxSize`.

### Note on `maxFreq` Handling

* `maxFreq` always stores the highest frequency of any character **seen so far** in the current window.
* Even if the actual max frequency in the window decreases after shrinking, we do **not** update `maxFreq` because the window will still shrink if invalid, ensuring correctness.
* This trade-off boosts performance and avoids re-scanning the frequency array.
* **Why not update `maxFreq` when the window shrinks?**

  * Because doing so might reduce its value prematurely and result in shorter window sizes than possible.
  * The key observation is: as long as `maxFreq` is the maximum **ever seen** in the current window, the maximum length can still be correctly computed, even if the actual frequency decreases.
  * Letting it drop would cause unnecessary shrinking and undercounting the valid window.
* If the current window becomes invalid, we shrink it, but we **already stored** the valid window length that was computed using the highest `maxFreq`. Hence, the recorded window size remains correct.

### Dry Run Example

**Input:** `s = "AABABBA", k = 1`

Step-by-step (right pointer moves):

* Window: A → maxFreq=1 → valid → maxSize=1
* Window: AA → maxFreq=2 → valid → maxSize=2
* Window: AAB → maxFreq=2 → valid → maxSize=3
* Window: AABA → maxFreq=3 → valid → maxSize=4
* Window: AABAB → maxFreq=3 → valid → maxSize=5
* Window: AABABB → maxFreq=3 → invalid (needs 2 changes) → shrink window
* Final maxSize = 4

### Code (Optimized - Java)

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int left = 0;
        int maxSize = 0;
        int maxFreq = 0;

        for(int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            freq[ch - 'A']++;
            maxFreq = Math.max(maxFreq, freq[ch - 'A']);

            if ((right - left + 1) - maxFreq > k) {
                char leftChar = s.charAt(left);
                freq[leftChar - 'A']--;
                left++;
            }

            maxSize = Math.max(maxSize, right - left + 1);
        }

        return maxSize;
    }
}
```

### Time and Space Complexity

* **Time Complexity:** `O(N)` where `N` is the length of the string. Each character is visited at most twice.
* **Space Complexity:** `O(1)` since the frequency array has a fixed size of 26.

---

## Approach 2: Brute Force with HashMap

This approach also uses a sliding window, but instead of maintaining a fixed-size frequency array, it uses a `HashMap` to track character counts in the window.

The key difference is that the **maximum frequency character in the window** is recalculated from the HashMap in every iteration, making this approach less efficient.

### Code (Brute Force - Java)

```java
class Solution {
    public int characterReplacement(String s, int k) {
        // character frequency map
        Map<Character,Integer> map = new HashMap<>();
        int left = 0;
        int maxSize = 0;
        // traverse a string by right pointer
        for(int right = 0; right < s.length(); right++) {
            // update the frequency of character
            char ch = s.charAt(right);
            map.put(ch, map.getOrDefault(ch, 0) + 1);

            // find the max frequency in the current window
            int maxFreq = Collections.max(map.entrySet(), Map.Entry.comparingByValue()).getValue();

            // check difference of current window size and maxFreq
            if ((right - left + 1) - maxFreq > k) {
                char leftChar = s.charAt(left);
                map.put(leftChar, map.get(leftChar) - 1);
                left++;
            }

            maxSize = Math.max(maxSize, right - left + 1);
        }

        return maxSize;
    }
}
```

### Time and Space Complexity

* **Time Complexity:** `O(N * 26)` in the worst case due to repeated scans for max frequency.
* **Space Complexity:** `O(26)` or `O(1)` due to bounded character set.

---

## Summary

Both approaches use a sliding window strategy. The optimized version avoids recalculating the max frequency by maintaining it separately, making it suitable for large strings. The brute-force version is easier to understand but less performant for long inputs.
