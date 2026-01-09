
# 🌳 LeetCode: Path Sum (Binary Tree)

## 📌 Problem Statement

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf path** such that **adding up all the values** along the path equals `targetSum`.

A **leaf** is a node with no children.

---

## 🧪 Example

### Tree:
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \       \
7    2       1
```

### Case 1:
- **targetSum = 22**
- Path: 5 → 4 → 11 → 2 → sum = 22 ✅
- **Return: true**

### Case 2:
- **targetSum = 26**
- Path: 5 → 8 → 13 → sum = 26 ✅
- **Return: true**

### Case 3:
- **targetSum = 18**
- No such root-to-leaf path ➤ **Return: false**

---

## ✅ Approach

We use **Depth-First Search (DFS)** recursion to traverse each root-to-leaf path and subtract node values from `targetSum`.

### Steps:
1. If current node is `None`, return `False`.
2. Subtract current node value from `targetSum`.
3. If current node is a **leaf** and remaining `targetSum` is 0 → return `True`.
4. Recursively check left and right subtrees.

---

## 🔍 Code (Python)
```python
def hasPathSum(root, targetSum):
    if not root:
        return False

    targetSum -= root.val

    if not root.left and not root.right:  # it's a leaf
        return targetSum == 0

    return hasPathSum(root.left, targetSum) or hasPathSum(root.right, targetSum)
```

---

## 🧠 Intuition

We keep subtracting node values from `targetSum` while going down the tree. If we reach a leaf and the remaining sum is `0`, it means we've found a valid path.

---

## 📈 Complexity

- **Time Complexity**: O(n) — we visit each node once.
- **Space Complexity**: O(h) — where `h` is the height of the tree (recursion stack).

---

## 💡 Notes

- Problem checks for **root-to-leaf** paths only.
- Be careful not to return `True` on intermediate nodes — only at leaf level.
