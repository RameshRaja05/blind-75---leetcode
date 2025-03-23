**Problem Statement:**
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine if the input string is **valid**.

A string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every closing bracket must have a corresponding open bracket of the same type.

---

**Constraints:**
- `1 <= s.length <= 10^4`
- `s` consists of parentheses only `'()[]{}'`.

---

**Example 1:**
```
Input: s = "()"
Output: true
```

**Example 2:**
```
Input: s = "()[]{}"
Output: true
```

**Example 3:**
```
Input: s = "(]"
Output: false
```

**Example 4:**
```
Input: s = "([])"
Output: true
```

---

**Approach:**
1. **Stack Data Structure:** Use a stack to track opening brackets.
2. **Iterate Through String:**
   - If the character is an opening bracket (`'('`, `'{'`, `'['`), push it onto the stack.
   - If it is a closing bracket (`')'`, `'}'`, `']'`), check:
     - If the stack is empty, return `False` (no matching opening bracket).
     - If the top of the stack is not the matching opening bracket, return `False`.
     - Otherwise, pop the stack.
3. **Final Check:** After iterating through `s`, the stack should be empty for a valid string.

This approach ensures an **O(n) time complexity** and **O(n) space complexity** due to stack usage.

---

**Python Solution:**
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for i in s:
            if i in "({[":
                stack.append(i)
            else:
                if not stack or \
                   (i == ")" and stack[-1] != "(") or \
                   (i == "}" and stack[-1] != "{") or \
                   (i == "]" and stack[-1] != "["):
                    return False
                stack.pop()
        return len(stack) == 0
```

---

**Time Complexity:**
- **O(n)**: We traverse the string once, and each character is pushed/popped at most once.

**Space Complexity:**
- **O(n)**: In the worst case (e.g., "((((((((((((((("), all characters are stored in the stack.

This method efficiently verifies whether the given string has balanced parentheses.

