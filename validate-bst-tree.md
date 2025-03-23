**Problem Statement:**
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

Input: `root = [2,1,3]`
Output: `true`

**Example 2:**

Input: `root = [5,1,4,null,null,3,6]`
Output: `false`
Explanation: The root node's value is 5 but its right child's value is 4.

**Constraints:**
- The number of nodes in the tree is in the range `[1, 10^4]`.
- `-2^{31} <= Node.val <= 2^{31} - 1`

---

## Approach:
To validate a binary search tree, we need to ensure that each node follows the BST properties:
1. **Recursive Approach with Valid Range:**
   - Use a helper function that takes the node, a lower bound, and an upper bound.
   - The left subtree must have values in the range `(low, node.val)`.
   - The right subtree must have values in the range `(node.val, high)`.
   - If any node violates these rules, return `False`.

2. **Iterative Approach Using Stack:**
   - Use a stack to perform an in-order traversal while tracking valid min/max constraints.
   - Ensure that each node follows the BST rule during traversal.

---

## Solution (Recursive Approach):
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isValidBST(root):
    def helper(node, low=float('-inf'), high=float('inf')):
        if not node:
            return True
        if not (low < node.val < high):
            return False
        return helper(node.left, low, node.val) and helper(node.right, node.val, high)
    
    return helper(root)
```

**Time Complexity:** `O(n)`, where `n` is the number of nodes, since we visit each node once.

**Space Complexity:** `O(n)` for recursive call stack in the worst case (skewed tree).

---

## Alternative Approach (Iterative In-Order Traversal):
```python
def isValidBST(root):
    stack = []
    prev = None
    
    while stack or root:
        while root:
            stack.append(root)
            root = root.left
        root = stack.pop()
        if prev and root.val <= prev.val:
            return False
        prev = root
        root = root.right
    
    return True
```

**Time Complexity:** `O(n)`, as we visit each node once.

**Space Complexity:** `O(n)`, due to stack storage in the worst case.

---

## Conclusion:
- **Recursive method** is more intuitive and follows the BST constraints naturally.
- **Iterative method** is efficient and avoids deep recursion issues.
- Both approaches ensure `O(n)` time complexity, making them optimal for validating BSTs.

