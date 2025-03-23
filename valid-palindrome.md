**Problem Statement:**
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

---

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"  
**Output:** `true`  
**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**

**Input:** s = "race a car"  
**Output:** `false`  
**Explanation:** "raceacar" is not a palindrome.

**Example 3:**

**Input:** s = " "  
**Output:** `true`  
**Explanation:** `s` is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.

---

**Constraints:**
- `1 <= s.length <= 2 * 10⁵`
- `s` consists only of printable ASCII characters.

---

**Approach:**

We need to ignore non-alphanumeric characters and check if the string reads the same forward and backward.

### **Two-Pointer Approach (O(n) Time Complexity)**
1. Use two pointers: one starting from the left (`left`) and the other from the right (`right`).
2. Move the `left` pointer forward until it reaches an alphanumeric character.
3. Move the `right` pointer backward until it reaches an alphanumeric character.
4. Compare characters at `left` and `right`. If they do not match, return `False`.
5. Continue until `left` crosses `right`.
6. If the entire string is checked and all characters match, return `True`.

**Time Complexity:** O(N)  
**Space Complexity:** O(1)  

**Implementation (Python):**

```python
import re

def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1
    
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True
```

This approach efficiently checks for palindrome properties while ensuring optimal performance. 🚀

