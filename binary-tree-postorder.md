

---

# 📄 Binary Tree Postorder Traversal

## Problem Link
[LeetCode 145 - Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Problem Description
Given the `root` of a binary tree, return the **postorder traversal** of its nodes' values.

- **Postorder Traversal** means:
  - Visit **Left Subtree** ➔ **Right Subtree** ➔ **Root Node**

### Example
```text
Input:
    1
     \
      2
     /
    3

Output: [3,2,1]
```

---

# 🧠 Algorithm Explanation

## What is Postorder Traversal?
- Postorder = Left ➔ Right ➔ Node
- We first **traverse the left child**, then **right child**, and finally **process the node** itself.

## How to Solve
There are two popular ways:

### 1. Recursive Approach
- Very simple and straightforward.
- We use **function call stack** to handle traversal.

Steps:
1. Traverse left subtree.
2. Traverse right subtree.
3. Visit the current node (append value to result).

---

### 2. Iterative Approach (using stack)
- Simulate the recursion manually using a **stack**.
- Trick: **Modified Preorder (Root ➔ Right ➔ Left)** and then **reverse the result**.

Steps:
1. Initialize empty stack and result list.
2. Push root into stack.
3. Pop node, **append node value at start** (or push normally and reverse later).
4. Push left and right children into stack.
5. After all nodes are visited, **reverse** the result list.

---

# 🎯 Visual Walkthrough

For the tree:

```
    1
     \
      2
     /
    3
```

Traversal Steps:
- Start at node 1 → left is NULL → go right to node 2
- At node 2 → left is node 3 → go left
- At node 3 → no left/right → add 3
- Back to node 2 → add 2
- Back to node 1 → add 1

Thus, **Postorder: [3,2,1]**

---

## 🛠️ Recursive Solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        
        def dfs(node):
            if not node:
                return
            dfs(node.left)       # Traverse left
            dfs(node.right)      # Traverse right
            result.append(node.val)  # Visit node
        
        dfs(root)
        return result
```

---

## 🛠️ Iterative Solution

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        stack = [root]
        result = []
        
        while stack:
            node = stack.pop()
            result.append(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        
        return result[::-1]  # Reverse to get Left -> Right -> Root
```

---

# ⏳ Time and Space Complexity

| Approach  | Time Complexity | Space Complexity | Explanation |
|:---------:|:---------------:|:----------------:|:------------|
| Recursive | O(N)             | O(N)             | Visit each node once; recursion stack in worst case |
| Iterative | O(N)             | O(N)             | Visit each node once; stack and result storage |

---

# ✅ Key Takeaways
- **Postorder = Left ➔ Right ➔ Node**.
- Recursive is simple; iterative uses **reverse of a modified preorder**.
- Always practice both approaches!

---

Would you also like me to **add a diagram showing Stack behavior step-by-step** for the iterative method? 📘  
It looks super cool and useful for deep understanding! 🚀 (Just say yes!)