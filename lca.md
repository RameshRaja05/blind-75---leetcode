**Problem Statement:**
Given the root of a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

**Example 1:**

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8  
**Output:** 6  
**Explanation:** The LCA of nodes 2 and 8 is 6.  

**Example 2:**

**Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4  
**Output:** 2  
**Explanation:** The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.  

**Example 3:**

**Input:** root = [2,1], p = 2, q = 1  
**Output:** 2  

**Constraints:**
- The number of nodes in the tree is in the range [2, 10⁵].
- -10⁹ <= Node.val <= 10⁹  
- All Node.val are unique.
- p != q
- p and q will exist in the BST.  

---

**Approach:**

To determine the lowest common ancestor (LCA) in a Binary Search Tree (BST), we utilize the BST properties:
1. If both `p` and `q` are smaller than `root.val`, then LCA lies in the left subtree.
2. If both `p` and `q` are greater than `root.val`, then LCA lies in the right subtree.
3. If `p` and `q` are on different sides (or one of them is equal to `root`), then `root` is the LCA.

### **Iterative Approach (BFS)**

1. Start from the root node.
2. Traverse the BST:
   - If `p.val` and `q.val` are both less than `root.val`, move to `root.left`.
   - If `p.val` and `q.val` are both greater than `root.val`, move to `root.right`.
   - Otherwise, `root` is the LCA.
3. Return the LCA node.

**Time Complexity:** O(H) — H is the height of the BST, which is O(log N) for a balanced BST and O(N) for a skewed BST.  
**Space Complexity:** O(1) — No extra space is used apart from the function call stack.  

**Implementation (Python):**

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def lowestCommonAncestor(self, root: Optional[TreeNode], p: TreeNode, q: TreeNode) -> TreeNode:
        while root:
            if p.val < root.val and q.val < root.val:
                root = root.left
            elif p.val > root.val and q.val > root.val:
                root = root.right
            else:
                return root
        return None
```

---

### **Recursive Approach (DFS)**

1. If `root` is `None`, return `None`.
2. If both `p` and `q` are smaller than `root.val`, recurse on the left subtree.
3. If both `p` and `q` are greater than `root.val`, recurse on the right subtree.
4. Otherwise, return `root` as the LCA.

**Time Complexity:** O(H)  
**Space Complexity:** O(H) (due to recursive stack space)  

**Implementation (Python):**

```python
class Solution:
    def lowestCommonAncestor(self, root: Optional[TreeNode], p: TreeNode, q: TreeNode) -> TreeNode:
        if root is None:
            return None
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```

Both approaches efficiently find the LCA in a BST. The iterative approach is preferred for its space efficiency, while the recursive approach offers a more natural traversal pattern.

