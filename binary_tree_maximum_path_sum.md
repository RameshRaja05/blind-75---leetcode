
# 🌲 LeetCode: Binary Tree Maximum Path Sum

## 📌 Problem Statement

Given the `root` of a binary tree, return the **maximum path sum**.

A **path** is any sequence of nodes where each pair of connected nodes is joined by an edge. A node can appear **only once** in the path. The path **does not need to go through the root**.

---

## 🧪 Example

### Tree:
```
      -10
      /  \
     9   20
         / \
        15  7
```

- The path with maximum sum is: `15 → 20 → 7`
- Sum = `15 + 20 + 7 = 42` ✅
- **Return: 42**

---

## ✅ Approach

We use **post-order traversal** (bottom-up) and a recursive DFS to calculate the **maximum path sum** at each node.

### Idea:
- For each node, compute the **maximum gain** from its left and right children.
- The path can either:
  - Continue **through one child** (used for parent calculation), or
  - Create a new path that **splits** at the current node (left + node + right).
- Keep track of the **global max path sum** across all such splits.

---

## 🔍 Code (Python)
```python
class Solution:
    def maxPathSum(self, root):
        self.max_sum = float('-inf')

        def dfs(node):
            if not node:
                return 0

            left_gain = max(dfs(node.left), 0)
            right_gain = max(dfs(node.right), 0)

            current_max = node.val + left_gain + right_gain

            self.max_sum = max(self.max_sum, current_max)

            return node.val + max(left_gain, right_gain)

        dfs(root)
        return self.max_sum
```

```
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
class Solution {
    public int res = 0;
    public int maxPathSum(TreeNode root) {
        this.res = root.val;
        dfs(root);
        return this.res;
    }

    private int dfs(TreeNode root){
        // base case
        if(root == null){
            return 0;
        }
        // search both directions left and right
        int leftMax = dfs(root.left);
        int rightMax = dfs(root.right);
        // replace -ne values with 0
        leftMax = Math.max(leftMax, 0);
        rightMax = Math.max(rightMax, 0);

        // if split happens store the result to the global res value
        this.res = Math.max(this.res, root.val + leftMax + rightMax);
        // return the non-split result
        // this means split happens in parent before current node
        // one more splits not accepted
        return root.val + Math.max(leftMax, rightMax); 
    }
}
```

---

## 🧠 Intuition

At every node:
- We calculate the best gain from left and right.
- We track the best path sum **including both sides** for max update.
- We return **only one side** (left or right) upward to stay on a valid path.

---

## 📈 Complexity

- **Time Complexity**: O(n) — we visit each node once.
- **Space Complexity**: O(h) — recursion stack, where `h` is tree height.

---

## 💡 Notes

- Negative values are allowed — ignore any negative path by taking `max(0, gain)`.
- This is not a root-to-leaf problem — any connected nodes can form a path.
