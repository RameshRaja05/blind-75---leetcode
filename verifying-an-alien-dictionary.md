Here's your updated **Markdown documentation** for **"Verifying an Alien Dictionary"**, now including a detailed **algorithm explanation** section.

---

````markdown
# 🌌 LeetCode Problem: Verifying an Alien Dictionary

## 🧾 Problem Statement

You are given a list of words and a string representing the order of characters in an alien language.  
Return `true` if and only if the given words are sorted lexicographically according to the given alien dictionary.

---

## 🔢 Input
- `words` – a list of strings representing words in an alien language
- `order` – a string representing the character order of the alien language (length = 26)

## ✅ Output
- Return `true` if the words are sorted according to the alien dictionary; otherwise, return `false`.

---

## 📘 Example

**Input:**
```java
words = ["hello", "leetcode"]
order = "hlabcdefgijkmnopqrstuvwxyz"
````

**Output:**

```
true
```

**Explanation:**

* `h` comes before `l` in the alien dictionary, so `"hello"` comes before `"leetcode"` → ✅

---

## 🧠 Algorithm Explanation

1. **Create a mapping of alien characters to indices**
   Build a hash map that stores the index of each character in the alien dictionary. This allows constant-time comparison of character precedence.

2. **Iterate through each adjacent word pair**
   Compare each word with the one that follows it in the list.

3. **Compare characters one by one**

   * Loop through both words character by character.
   * If the characters differ, check their positions in the alien dictionary using the map.

     * If the first word’s character comes **after** the second word’s → not sorted → return `false`.
     * If it comes **before** → the order is valid → break comparison for this pair.
   * If all characters are the same and the first word is longer than the second → invalid → return `false`.

4. **If all pairs are valid**, return `true`.

---

## 🧮 Visual Example

```
words = ["app", "apple"]
alien_order = "abcdefghijklmnopqrstuvwxyz"

Comparison:
'app' vs 'apple' => Same chars: a, p, p → 'app' is shorter → valid ✅
```

---

## ✅ Java Code

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        Map<Character,Integer> orderInd = new HashMap<>();
        for(int i = 0; i < order.length(); i++) {
            orderInd.put(order.charAt(i), i);
        }

        for(int i = 0; i < words.length - 1; i++) {
            if (!inCorrectOrder(words[i], words[i + 1], orderInd)) {
                return false;
            }
        }

        return true;
    }

    private boolean inCorrectOrder(String word1, String word2, Map<Character,Integer> orderInd) {
        for(int j = 0; j < word1.length(); j++) {
            if (j == word2.length()) return false;

            char c1 = word1.charAt(j);
            char c2 = word2.charAt(j);

            if (c1 != c2) {
                return orderInd.get(c1) < orderInd.get(c2);
            }
        }
        return true;
    }
}
```

---

## 🔁 Dry Run

**Input:**

```java
words = ["apple", "app"]
order = "abcdefghijklmnopqrstuvwxyz"
```

**Comparison:**

* `'a' == 'a'`
* `'p' == 'p'`
* `'p' == 'p'`
* word2 ends, but word1 still has characters → `apple` > `app` → invalid → return `false`

---

## ⏱️ Time and Space Complexity

| Metric           | Complexity                                                            |
| ---------------- | --------------------------------------------------------------------- |
| Time Complexity  | `O(N * M)` where N is the number of words and M is the average length |
| Space Complexity | `O(1)` for the order map (fixed size 26)                              |

---

## 🧪 Test Cases

| Input                                                      | Output  |
| ---------------------------------------------------------- | ------- |
| `["hello", "leetcode"]`, `"hl...z"`                        | `true`  |
| `["word", "world", "row"]`, `"worldabcefghijkmnpqstuvxyz"` | `false` |
| `["apple", "app"]`, `"abcdefghijklmnopqrstuvwxyz"`         | `false` |
| `["kuvp", "q"]`, `"ngxlkthsjuoqcpavbfdermiywz"`            | `true`  |

---

```

Let me know if you'd like a template version you can reuse for other LeetCode problems!
```
