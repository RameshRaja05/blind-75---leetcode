**Problem Statement:**
Given the root of a binary tree, invert the tree, and return its root.

**Example 1:**

Input: `root = [4,2,7,1,3,6,9]`
Output: `[4,7,2,9,6,3,1]`

**Example 2:**

Input: `root = [2,1,3]`
Output: `[2,3,1]`

**Example 3:**

Input: `root = []`
Output: `[]`

**Constraints:**
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

---

## Approach:
To invert a binary tree, we need to swap the left and right children of every node.

1. **Recursive Approach:**
   - If the tree is empty (`root is None`), return `None`.
   - Swap the left and right child nodes recursively.
   - Return the root after inverting.

2. **Iterative Approach Using Queue (BFS):**
   - Use a queue to perform a level-order traversal.
   - Swap the left and right children at each node.
   - Continue until all nodes are processed.

---

## Solution (Recursive Approach):
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invertTree(root):
    if not root:
        return None
    root.left, root.right = invertTree(root.right), invertTree(root.left)
    return root
```

**Time Complexity:** `O(n)`, since each node is visited once.

**Space Complexity:** `O(h)`, where `h` is the height of the tree (`O(log n)` for balanced trees, `O(n)` for skewed trees).

---

## Solution (Iterative Approach Using BFS):
```python
from collections import deque

def invertTree(root):
    if not root:
        return None
    queue = deque([root])
    while queue:
        node = queue.popleft()
        node.left, node.right = node.right, node.left  # Swap children
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return root
```

**Time Complexity:** `O(n)`, as each node is processed once.

**Space Complexity:** `O(n)`, due to queue storage in the worst case.

---

## Conclusion:
- **Recursive method** is simpler and elegant but may cause stack overflow for deep trees.
- **Iterative method (BFS)** is more memory-efficient in shallow trees and avoids deep recursion issues.
- Both approaches provide an optimal `O(n)` time complexity.

