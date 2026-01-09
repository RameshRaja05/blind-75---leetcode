
# 🧠 LeetCode: Lowest Common Ancestor of a Binary Tree

## 📌 Problem Statement
Given the root of a binary tree and two nodes `p` and `q`, return their **lowest common ancestor (LCA)**.

The **LCA** of two nodes `p` and `q` is defined as the **lowest** node in the tree that has both `p` and `q` as **descendants** (where a node can be a descendant of itself).

---

## 🧪 Example

Given the binary tree:

```
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4
```

- `LCA(5, 1) = 3`
- `LCA(5, 4) = 5`

---

## ✅ Approach

We use a **bottom-up recursive** approach:

1. If the current node is `None`, return `None`.
2. If the current node is `p` or `q`, return the current node.
3. Recursively search in the left and right subtrees.
4. If both left and right return non-null, the current node is the LCA.
5. Otherwise, return the non-null result (either left or right).

---

## 🧠 Intuition

- We traverse the tree in **post-order** fashion.
- At each step, we ask:  
  "Does the left or right subtree contain `p` or `q`?"
- The **lowest** node at which both subtrees report one match each is the LCA.

---

## 🔍 Code (Python)
```python
def lowestCommonAncestor(root, p, q):
    if not root or root == p or root == q:
        return root

    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)

    if left and right:
        return root
    return left if left else right
```

---

## 🧪 Walkthrough (Case: `LCA(5, 4)`)

- Start at node `3`.
- Explore left subtree:  
  - At node `5` → match `p`
  - Recurse into `5`’s subtree and find `4`
- Both `5` and `4` are found in left subtree.
- So, the answer is `5`.

---

## 📈 Complexity

- **Time Complexity**: `O(n)` — we visit each node once.
- **Space Complexity**: `O(h)` — where `h` is the height of the tree (due to recursion stack).

---

## 🧩 Notes

- The tree is not a BST, so we **can’t** rely on value comparisons.
- Node values are unique, but we compare nodes **by reference**, not value.


---

## 🧠 My Understanding of the LCA Algorithm

1. **If `p` and `q` are found in separate subtrees** (one in the left and one in the right),  
   ➤ the **current node** is the **Lowest Common Ancestor (LCA)**.

2. **If both `p` and `q` are found in the same subtree** (both in left or both in right),  
   ➤ the recursive call for that subtree will return the **non-null node**, which will bubble up as the LCA.

3. This way, the **nearest common parent** (lowest in the tree) is returned as the LCA.

---

### 🧪 Example Walkthrough

Consider this binary tree:

```
      3
     / \
    2   5
   / \
  9   7
```

Let’s find the **LCA of nodes 9 and 7**.

- Start at root `3`
  - Search the **left subtree**:
    - Reach node `2`
      - Left child is `9` → match ✅
      - Right child is `7` → match ✅
    - Since both children matched, node `2` is the LCA of `9` and `7`
  - Search the **right subtree** of root:
    - It returns `null` because neither `9` nor `7` exist there

✅ So, the final answer is **node `2`**

---

### 🧠 Summary

This algorithm works using **post-order traversal** (bottom-up), and the logic is:
- If both sides return non-null → current node is LCA.
- If only one side is non-null → bubble that result up.
- If current node is `p` or `q`, return it immediately.
