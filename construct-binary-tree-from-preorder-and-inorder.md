Got it!  
I'll document everything cleanly in Markdown, **add the Java approach** too, and keep it neat for your future reference. Here’s the complete Markdown file 👇:

---

# 📄 Construct Binary Tree from Preorder and Inorder Traversal

## Problem Link
[LeetCode 105 - Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

---

## 📜 Problem Description
Given two integer arrays `preorder` and `inorder`, where:

- `preorder` is the preorder traversal of a binary tree.
- `inorder` is the inorder traversal of the same tree.

**Construct and return** the binary tree.

---

## ✏️ Example
```text
Input:
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]

Output:
    3
   / \
  9  20
    /  \
   15   7
```

---

# 🧠 Algorithm Explanation

## How Preorder and Inorder Help?

- **Preorder**: [**Root**, Left, Right]
  - First element = **Root** node.

- **Inorder**: [Left, **Root**, Right]
  - Find root in `inorder` ➔ everything left = left subtree, right = right subtree.

---

## Step-by-Step Plan

1. Initialize a **global pointer** for `preorder` array (to pick the next root).
2. Create a **HashMap** for `inorder` for **O(1)** lookup of root index.
3. Build a recursive function:
   - If bounds are crossed ➔ return `null`.
   - Create root from `preorder`.
   - Find the root’s index in `inorder`.
   - Recurse:
     - Left subtree: from current `left` bound to `root index - 1`.
     - Right subtree: from `root index + 1` to current `right` bound.

---

# 🎯 Visual Walkthrough

Given:

```text
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
```

### Building Steps:
1. **preorder[0] = 3**, root = 3
2. In `inorder`, 3 is at index 1:
   - left subtree → [9]
   - right subtree → [15,20,7]

3. Now left child:
   - **preorder[1] = 9**, root = 9
   - No left/right since only one element [9].

4. Now right child:
   - **preorder[2] = 20**, root = 20
   - In `inorder`, 20 splits into [15] and [7]

5. Left child of 20:
   - **preorder[3] = 15**, root = 15

6. Right child of 20:
   - **preorder[4] = 7**, root = 7

### Final Tree:
```
      3
     / \
    9   20
        / \
       15  7
```

---

# 🛠️ Python Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        inorder_index_map = {val: idx for idx, val in enumerate(inorder)}
        self.preorder_index = 0
        
        def array_to_tree(left, right):
            if left > right:
                return None
            
            root_val = preorder[self.preorder_index]
            root = TreeNode(root_val)
            self.preorder_index += 1
            
            root.left = array_to_tree(left, inorder_index_map[root_val] - 1)
            root.right = array_to_tree(inorder_index_map[root_val] + 1, right)
            
            return root
        
        return array_to_tree(0, len(inorder) - 1)
```

---

# 🛠️ Java Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
import java.util.*;

class Solution {
    private int preorderIndex;
    private Map<Integer, Integer> inorderIndexMap;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderIndex = 0;
        inorderIndexMap = new HashMap<>();
        
        // Build hashmap for inorder values
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        
        return arrayToTree(preorder, 0, inorder.length - 1);
    }
    
    private TreeNode arrayToTree(int[] preorder, int left, int right) {
        if (left > right) {
            return null;
        }
        
        // Pick up preorderIndex element as a root
        int rootValue = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootValue);
        
        // Build left and right subtree
        root.left = arrayToTree(preorder, left, inorderIndexMap.get(rootValue) - 1);
        root.right = arrayToTree(preorder, inorderIndexMap.get(rootValue) + 1, right);
        
        return root;
    }
}
```

---

# ⏳ Time and Space Complexity

| Approach       | Time Complexity | Space Complexity | Reason |
|:--------------:|:----------------:|:----------------:|:------:|
| Recursive + HashMap | O(N)         | O(N)              | Each node processed once + storing indices. |

---

# ✅ Key Takeaways
- **Preorder first element** always points to the root.
- **Inorder split** helps divide left and right subtrees.
- Use a **HashMap** for fast index lookup ➔ reduces time from O(N²) to O(N).
- **Recursive tree building** is an important pattern in Binary Trees.

---

Would you also like me to prepare a **diagram showing each recursion call frame**, like a recursion tree? 🌳  
It’s super helpful if you're planning to master tree-building questions! (Just say "yes!") 🚀