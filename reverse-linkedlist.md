# Problem: Reverse a Singly Linked List

## Problem Statement:
Given the head of a singly linked list, reverse the list, and return the reversed list.

### Example 1:
#### Input:
head = [1,2,3,4,5]
#### Output:
[5,4,3,2,1]

### Example 2:
#### Input:
head = [1,2]
#### Output:
[2,1]

### Example 3:
#### Input:
head = []
#### Output:
[]

### Constraints:
- The number of nodes in the list is in the range [0, 5000].
- -5000 <= Node.val <= 5000

### Follow-up:
A linked list can be reversed either iteratively or recursively. Could you implement both?

---

## Approach 1: Iterative Method
### Algorithm:
1. Initialize three pointers: `prev` (initially `null`), `current` (pointing to head), and `next` (to track the next node).
2. Iterate through the list:
   - Store the next node in `next`.
   - Reverse the `current` node’s pointer to point to `prev`.
   - Move `prev` and `current` one step forward.
3. After exiting the loop, `prev` will be the new head of the reversed list.

### Code (Java):
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode next = current.next; // Store next node
            current.next = prev; // Reverse current node's pointer
            prev = current; // Move prev forward
            current = next; // Move current forward
        }
        return prev; // New head
    }
}
```

### Time Complexity:
- **O(N)**, where N is the number of nodes in the linked list, as we traverse the list once.

### Space Complexity:
- **O(1)**, as we only use a few pointers.

---

## Approach 2: Recursive Method
### Algorithm:
1. Base case: If `head` is `null` or has only one node, return `head`.
2. Recursively reverse the rest of the list.
3. Adjust the pointers of the reversed part to make the current node the new tail.
4. Return the new head.

### Code (Java):
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

### Time Complexity:
- **O(N)**, as we traverse each node once.

### Space Complexity:
- **O(N)**, due to recursive stack space.

---

## Summary:
| Method       | Time Complexity | Space Complexity |
|-------------|----------------|------------------|
| Iterative   | O(N)            | O(1)             |
| Recursive   | O(N)            | O(N) (stack)     |

The **iterative approach** is generally preferred for its **constant space complexity**, while the **recursive approach** provides a more intuitive understanding of how the linked list is reversed.