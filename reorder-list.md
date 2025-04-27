---

# 🔁 Reorder List – Linked List Algorithm

## 📝 Problem

Given a singly linked list:

```
L0 → L1 → L2 → ... → Ln
```

Reorder it into:

```
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
```

Only **relink nodes**, don’t change their values.

---

## 🚶‍♂️ Step-by-Step Approach (Three Phases)

### ✅ Phase 1: Find the Middle Node (using fast & slow pointer)
We want to **split the list in half**.

- Use the **tortoise and hare** approach:
  - `slow` moves 1 step
  - `fast` moves 2 steps
  - When `fast` reaches the end, `slow` will be at mid

### ✅ Phase 2: Reverse the Second Half
Reverse the second part of the list starting from `slow.next`.

### ✅ Phase 3: Merge Two Halves
Alternate nodes from the first half and reversed second half.

---

## 💻 Java Code Implementation

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;

        // Step 1: Find the middle
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Step 2: Reverse second half
        ListNode second = reverse(slow.next);
        slow.next = null; // Cut the list into two halves

        // Step 3: Merge the two halves
        ListNode first = head;
        while (second != null) {
            ListNode temp1 = first.next;
            ListNode temp2 = second.next;

            first.next = second;
            second.next = temp1;

            first = temp1;
            second = temp2;
        }
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
}
```

---

## 📊 Complexity

| Metric | Value        |
|--------|--------------|
| Time   | O(n)         |
| Space  | O(1) (in-place) |

---

## 📈 Example Walkthrough

Input:
```
1 → 2 → 3 → 4 → 5
```

**Phase 1** (mid at 3):
```
First Half: 1 → 2 → 3
Second Half: 4 → 5
```

**Phase 2** (reverse second half):
```
Second Half becomes: 5 → 4
```

**Phase 3** (merge):
```
1 → 5 → 2 → 4 → 3
```

---

## 📌 Visual Representation

```
Original:
1 → 2 → 3 → 4 → 5

After splitting:
1 → 2 → 3      4 → 5

After reversing second:
1 → 2 → 3      5 → 4

Final merge:
1 → 5 → 2 → 4 → 3
```

---

Would you like to visualize this with a flowchart or include a test utility method in Java for verification?

