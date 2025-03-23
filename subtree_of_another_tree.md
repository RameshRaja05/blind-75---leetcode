Here’s the approach to solving this problem efficiently:  

### **Approach:**
1. **Check if Subtree Exists**  
   - Start from the root of the main tree and check if the `subRoot` is identical to any subtree.
   - Recursively traverse the left and right subtrees to continue checking.

2. **Check Tree Equality (Helper Function `isSameTree`)**  
   - Compare if two trees are identical in structure and values.
   - If both trees are `None`, return `True`.
   - If one tree is `None` and the other is not, return `False`.
   - If the root values don’t match, return `False`.
   - Recursively check the left and right children.

---

### **Python Implementation**
```python
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root:
            return False
        
        if self.isSameTree(root, subRoot):
            return True
        
        # Recursively check left and right subtrees
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
    
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val != q.val:
            return False
        
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

---

### **Time Complexity Analysis:**
- **`isSameTree`** takes **O(N)** in the worst case (where `N` is the number of nodes in `root`).
- **`isSubtree`** checks `isSameTree` at every node of `root`, leading to **O(M * N)** complexity (where `M` is the number of nodes in `root` and `N` is the number of nodes in `subRoot`).

Let me know if you want further optimizations! 🚀