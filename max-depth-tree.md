**Problem Statement:**
Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

**Input:** root = [3,9,20,null,null,15,7]  
**Output:** 3  

**Example 2:**

**Input:** root = [1,null,2]  
**Output:** 2  

**Constraints:**
- The number of nodes in the tree is in the range [0, 10⁴].
- -100 <= Node.val <= 100  

---

**Approach:**

To determine the maximum depth of a binary tree, we can use **Depth-First Search (DFS)** with recursion or **Breadth-First Search (BFS)** with a queue.

### **DFS Approach:**

1. If the root is `null`, return `0` as the depth.
2. Recursively compute the depth of the left and right subtrees.
3. The maximum depth is `1 + max(leftDepth, rightDepth)`, where `1` accounts for the current node.

**Algorithm:**
- Base case: If the root is `null`, return `0`.
- Recursively call the function on the left and right subtrees.
- Take the maximum of the two depths and add `1`.

**Time Complexity:** O(N) — We visit each node once.  
**Space Complexity:** O(H) — In the worst case (skewed tree), recursion depth can go up to H (height of the tree), i.e., O(N) in the worst case and O(log N) for a balanced tree.

**Implementation (Python):**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root):
    if not root:
        return 0
    
    left_depth = maxDepth(root.left)
    right_depth = maxDepth(root.right)
    
    return 1 + max(left_depth, right_depth)
```

---

### **BFS Approach (Your Approach):**

1. If the root is `None`, return `0`.
2. Use a queue to perform level-order traversal.
3. Initialize the queue with the root node and maintain a `depth` variable.
4. For each level, process all nodes at that level, then increment `depth`.
5. Continue until all nodes are processed.

**Time Complexity:** O(N) — Each node is visited once.  
**Space Complexity:** O(N) — In the worst case, we may store all nodes in the queue.

**Implementation (Python):**

```python
from collections import deque
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        queue = deque([root])
        depth = 0
        while queue:
            for i in range(len(queue)):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            depth += 1
        return depth
```

Both approaches effectively compute the maximum depth of a binary tree, but DFS is a recursive method, while BFS is an iterative approach using a queue.