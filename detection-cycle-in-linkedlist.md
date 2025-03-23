**Problem Statement:**
Given `head`, the head of a linked list, determine if the linked list contains a cycle.

A cycle occurs if there is a node in the list that can be reached again by continuously following the `next` pointer. The `pos` variable is used internally to denote the index where the cycle begins but is not passed as a parameter.

Return `true` if a cycle exists; otherwise, return `false`.

---

**Constraints:**
- The number of nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a valid index in the linked list.

---

**Example 1:**
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

---

**Approach:**
To detect a cycle efficiently, we use **Floyd’s Cycle Detection Algorithm (Tortoise and Hare Approach)**.

**Algorithm:**
1. Initialize two pointers:
   - `slow` moves one step at a time.
   - `fast` moves two steps at a time.
2. If there is a cycle, `fast` will eventually meet `slow` inside the cycle.
3. If `fast` reaches the end (`None`), then there is no cycle.
4. Return `true` if `slow` and `fast` meet; otherwise, return `false`.

---

**Python Solution:**
```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast, slow = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True
        return False
```

---

**Time Complexity:**
- **O(n)**: In the worst case, both pointers traverse the linked list once.

**Space Complexity:**
- **O(1)**: No extra memory is used.

This approach ensures an optimal and efficient solution for detecting cycles in a linked list.

