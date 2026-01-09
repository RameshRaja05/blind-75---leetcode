# LeetCode 23: Merge k Sorted Lists

## 📝 Problem Statement

Merge `k` sorted linked lists and return it as one sorted list.

All the linked lists are sorted in **ascending** order.

**Function Signature:**
```java
public ListNode mergeKLists(ListNode[] lists)
```

---

## 🧠 Algorithm Intuition

To merge `k` sorted linked lists efficiently, we can apply the **divide and conquer** approach:

- Repeatedly merge pairs of lists until there is only **one** list remaining.
- This is similar to the **merge step in merge sort**.

---

## 📌 Algorithm Steps

1. **Handle edge cases**:
   - If `lists` is null or empty, return `null`.

2. **While there's more than one list**:
   - Initialize a temporary list `temp` to store merged results.
   - Iterate over `lists` in steps of 2.
     - For each pair (`l1`, `l2`), merge them using a helper `mergeTwoLists` method.
     - Append the merged list to `temp`.
   - Update `lists = temp`.

3. **Return the remaining list**:
   - When only one list remains in `lists`, return `lists[0]`.

---

## 🔧 Helper Function: `mergeTwoLists(l1, l2)`

This function merges two sorted linked lists:

- Use a dummy node to simplify list merging.
- Compare values from both lists and append the smaller node.
- If one list is exhausted, append the rest of the other list.

---

## 📍 Dry-Run Example

### Input:

```java
lists = [
  1 -> 4 -> 5,
  1 -> 3 -> 4,
  2 -> 6
]
```

### First Round (Pairwise merge):
- Merge `lists[0]` and `lists[1]`:
  - Result: `1 -> 1 -> 3 -> 4 -> 4 -> 5`
- Merge with `lists[2]`:
  - Result: `1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6`

Final Output:
```text
1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

---

## ⏱️ Time and Space Complexity

### Time Complexity:
- Each merge operation takes `O(n)` time where `n` is the total number of nodes.
- Total number of merges: `log k` levels (like merge sort).
- So, **Overall: `O(N log k)`** where `N` is the total number of nodes in all lists.

### Space Complexity:
- **O(1)** auxiliary (in-place merging).
- If recursion is used in `mergeTwoLists`, space is `O(k)` due to call stack.
- Here, iteration is used → optimal space.

---

## ✅ Java Code

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }

        while (lists.length > 1) {
            List<ListNode> temp = new ArrayList<>();

            for (int i = 0; i < lists.length; i += 2) {
                ListNode l1 = lists[i];
                ListNode l2 = (i + 1 < lists.length) ? lists[i + 1] : null;
                temp.add(mergeTwoLists(l1, l2));
            }

            lists = temp.toArray(new ListNode[0]);
        }

        return lists[0];
    }

    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }

        tail.next = (l1 != null) ? l1 : l2;

        return dummy.next;
    }
}
```

---

## 🔚 Conclusion

This divide-and-conquer approach is efficient and scalable for large values of `k`. It avoids the naive approach of sequential merging, which can be slow.


---

## 🚀 Alternative Approach: Min Heap (Priority Queue)

### 🧠 Intuition

- We use a **min-heap** (priority queue) to always retrieve the node with the smallest value among the current heads of all lists.
- Initially, insert the **head node of each non-empty list** into the heap.
- Repeatedly extract the smallest node, attach it to the merged list, and push its `next` node (if any) into the heap.
- This ensures we always add the smallest node available across all current list heads.

---

### 🔄 Algorithm Steps

1. **Initialize a min-heap** to store `ListNode` objects.
2. Use a **custom comparator** to sort `ListNode`s by their `val`.
3. **Insert the head node** of each non-null linked list into the heap.
4. Create a **dummy node** and let `tail` point to it for building the merged list.
5. While the heap is not empty:
   - Pop the **smallest node** (`minNode`) from the heap.
   - Append it to the merged list: `tail.next = minNode`
   - Move `tail` forward: `tail = tail.next`
   - If `minNode.next` is not null, push it to the heap.
6. Return `dummy.next`.

---

### 📍 Dry-Run Example

#### Input:
```java
lists = [
  1 -> 4 -> 5,
  1 -> 3 -> 4,
  2 -> 6
]
```

**Heap Initialization:**
- Heap contains nodes with values: `[1, 1, 2]`

**Process:**
- Pop 1 → add to result → push 4
- Pop 1 → add to result → push 3
- Pop 2 → add to result → push 6
- Pop 3 → add to result
- Pop 4 → add to result → push 5
- Pop 4 → add to result
- Pop 5 → add to result
- Pop 6 → add to result

**Output:**  
`1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6`

---

### ✅ Java Code (Heap-Based)

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;

        PriorityQueue<ListNode> minHeap = new PriorityQueue<>(
            (a, b) -> Integer.compare(a.val, b.val)
        );

        for (ListNode node : lists) {
            if (node != null) minHeap.add(node);
        }

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (!minHeap.isEmpty()) {
            ListNode minNode = minHeap.poll();
            tail.next = minNode;
            tail = tail.next;

            if (minNode.next != null) {
                minHeap.add(minNode.next);
            }
        }

        return dummy.next;
    }
}
```

---

### ⏱️ Time and Space Complexity

**Time Complexity:** `O(N log k)`  
- `N` is the total number of nodes.
- Each insertion and removal from heap takes `O(log k)`, repeated `N` times.

**Space Complexity:** `O(k)`  
- Heap stores at most `k` nodes at any time.

---
