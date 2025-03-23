**Problem Statement:**
Given the heads of two sorted linked lists `list1` and `list2`, merge them into one sorted linked list. The merged list should be created by splicing together the nodes of the two input lists.

**Return:** The head of the merged linked list.

---

**Constraints:**
- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing order**.

---

**Example 1:**
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**
```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**
```
Input: list1 = [], list2 = [0]
Output: [0]
```

---

**Approach:**
1. **Iterative Merge Using a Dummy Node:**
   - Create a dummy node to simplify handling edge cases.
   - Maintain a pointer `current` starting at the dummy node.
   - Traverse both lists:
     - Compare the current nodes of `list1` and `list2`.
     - Append the smaller node to the `current` pointer.
     - Move the pointer forward in the respective list.
   - If any nodes remain in either list, append them directly.
   - Return the merged list starting from `dummy.next`.

2. **Recursive Merge:**
   - Base cases: If one list is empty, return the other list.
   - Compare the head nodes and recursively merge the remaining lists.

---

**Python Solution (Iterative):**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        dummy = ListNode(-1)
        current = dummy
        
        while list1 and list2:
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next
        
        # Append any remaining nodes
        current.next = list1 if list1 else list2
        
        return dummy.next
```

---

**Python Solution (Recursive):**
```python
class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        if not list1:
            return list2
        if not list2:
            return list1
        
        if list1.val < list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```

---

**Time Complexity:**
- **O(n + m)**: `n` and `m` are the lengths of `list1` and `list2`. Each node is processed once.

**Space Complexity:**
- **O(1) for Iterative**: Uses constant extra space.
- **O(n + m) for Recursive**: Due to recursive stack calls.

This ensures an efficient and optimal solution for merging two sorted linked lists.

