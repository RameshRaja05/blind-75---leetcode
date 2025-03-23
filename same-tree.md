**Problem Statement:**
Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are:
1. Structurally identical.
2. The nodes have the same value.

**Example 1:**

Input: `p = [1,2,3]`, `q = [1,2,3]`
Output: `true`

**Example 2:**

Input: `p = [1,2]`, `q = [1,null,2]`
Output: `false`

**Example 3:**

Input: `p = [1,2,1]`, `q = [1,1,2]`
Output: `false`

**Constraints:**
- The number of nodes in both trees is in the range `[0, 100]`.
- `-10^4 <= Node.val <= 10^4`

---

## Approach:
To determine if two binary trees are the same, we need to compare:
1. Their root values.
2. Their left subtrees.
3. Their right subtrees.

We can achieve this using:
1. **Recursive Approach:**
   - If both trees are `None`, return `True`.
   - If only one of them is `None`, return `False`.
   - If root values differ, return `False`.
   - Recursively check the left and right subtrees.

2. **Iterative Approach Using Queue (BFS):**
   - Use a queue to compare corresponding nodes level by level.
   - If structures differ at any level, return `False`.

---

## Solution (Recursive Approach):
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSameTree(p, q):
    if not p and not q:
        return True
    if not p or not q or p.val != q.val:
        return False
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

**Time Complexity:** `O(n)`, where `n` is the number of nodes, since we visit each node once.

**Space Complexity:** `O(h)`, where `h` is the height of the tree (`O(log n)` for balanced trees, `O(n)` for skewed trees).

---

## Solution (Iterative Approach Using BFS):
```python
from collections import deque

def isSameTree(p, q):
    queue = deque([(p, q)])
    while queue:
        node1, node2 = queue.popleft()
        if not node1 and not node2:
            continue
        if not node1 or not node2 or node1.val != node2.val:
            return False
        queue.append((node1.left, node2.left))
        queue.append((node1.right, node2.right))
    return True
```

**Time Complexity:** `O(n)`, as each node is compared once.

**Space Complexity:** `O(n)`, due to queue storage in the worst case.

---

## Conclusion:
- **Recursive method** is simpler and easier to understand.
- **Iterative method (BFS)** is useful for avoiding deep recursion issues.
- Both approaches have `O(n)` time complexity and are optimal for this problem.

