# 🧹 LeetCode: Group Anagrams

**Problem Link:** [Group Anagrams - LeetCode](https://leetcode.com/problems/group-anagrams/)
**Difficulty:** Medium
**Tags:** Hash Table, String, Sorting

---

## 📝 Problem Summary

Given an array of strings `strs`, group the anagrams together.
You can return the answer in **any order**.

An **anagram** is a word or phrase formed by rearranging the letters of another, using all original letters exactly once.

### 🧠 Example:

```text
Input:  strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["eat","tea","ate"],["tan","nat"],["bat"]]
```

---

## ✅ Constraints:

* `1 <= strs.length <= 10⁴`
* `0 <= strs[i].length <= 100`
* `strs[i]` consists of lowercase English letters.

---

## 💡 Approach

### 🔹 Intuition:

Anagrams, when sorted alphabetically, become the same string.
So we can group words by their **sorted version**.

### 🔹 Strategy:

* Use a hash map to map sorted words to a list of original words.
* Iterate over the list, sort each word, and store it as the key.
* Append the original word to the group for that key.

---

## 🧪 Algorithm Steps (Step-by-Step Walkthrough)

1. Initialize a map to store grouped anagrams.
2. Loop through each string `s` in the input list `strs`.
3. Sort the string: `sortedS = sortChars(s)`
4. Use `sortedS` as the key to group anagrams.
5. Return the list of grouped values.

---

## 🧑‍💻 Code (Java):

```java
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) return new ArrayList<>();

        Map<String, List<String>> map = new HashMap<>();

        for (String s : strs) {
            char[] charArray = s.toCharArray();
            Arrays.sort(charArray);
            String key = new String(charArray);

            map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }

        return new ArrayList<>(map.values());
    }
}
```

---

## ⏱️ Time & Space Complexity

| Complexity | Value                                                               |
| ---------- | ------------------------------------------------------------------- |
| Time       | O(N \* K log K), where N = number of words, K = average word length |
| Space      | O(N \* K) — for storing grouped words in the map                    |

---

## 📌 Alternate Approach (Using Character Count as Key - Python Example)

```python
from collections import defaultdict

def groupAnagrams(strs):
    anagram_map = defaultdict(list)

    for s in strs:
        count = [0] * 26
        for c in s:
            count[ord(c) - ord('a')] += 1
        anagram_map[tuple(count)].append(s)

    return list(anagram_map.values())
```

### 🔎 Algorithm Explanation:

* Each word is represented by a **26-element frequency count** (for 'a' to 'z').

* Words with the same character frequency will produce the same tuple.

* This tuple is used as the **hash key** in a dictionary.

* Anagrams end up in the same list because they produce identical character count tuples.

* **Benefit:** Faster than sorting when string length is large, as this approach runs in O(K) per word instead of O(K log K).

* **Key Type:** Tuples are immutable and hashable, making them ideal for dictionary keys.

### 💻 Java Version of Alternate Approach:

```java
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();

        for (String s : strs) {
            int[] count = new int[26];
            for (char c : s.toCharArray()) {
                count[c - 'a']++;
            }
            // Convert count array to a unique string key
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; i++) {
                sb.append('#'); // separator
                sb.append(count[i]);
            }
            String key = sb.toString();
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }

        return new ArrayList<>(map.values());
    }
}
```

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,List<String>> map = new HashMap<>();

        for(String str:strs){
            // freq list used as a key
            char[] freq = new char[26];
            // compute the freq list
            for(char ch:str.toCharArray()){
                freq[ch - 'a']++;
            }

            String key = new String(freq);

            List<String> list = map.getOrDefault(key, new ArrayList<>());

            list.add(str);
            
            map.put(key, list);
        }

        return new ArrayList<>(map.values());
    }
}
```

* Instead of sorting, this uses a frequency array encoded into a unique string (e.g., `#1#0#0#0...`) as the key.
* This saves time and is effective for longer strings.

---
