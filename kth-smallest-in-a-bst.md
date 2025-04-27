Sure! Below is a documentation template for solving the problem of finding the Kth smallest element in a Binary Search Tree (BST).

---

# LeetCode Problem: Kth Smallest Element in a BST

## Problem Statement

> Given a Binary Search Tree (BST), write a function to find the Kth smallest element in it. You are guaranteed that the BST has at least `k` elements.

### Example 1:
Input:  
```
root = [3, 1, 4, null, 2]
k = 1
```
Output:  
```
1
```

### Example 2:
Input:  
```
root = [5, 3, 6, 2, 4, null, null, 1]
k = 3
```
Output:  
```
3
```

## Approach

1. **Understand the Structure of a BST**:  
   - In a BST, the left subtree contains only nodes with values less than the parent node, and the right subtree only contains nodes with values greater than the parent node.
   - In-order traversal of a BST gives the elements in sorted order (ascending).

2. **In-order Traversal**:  
   - Perform an in-order traversal of the BST. The in-order traversal of a BST will visit nodes in ascending order.
   - The Kth smallest element will be the Kth node encountered during the in-order traversal.

3. **Optimized Approach**:  
   - Use a counter during the in-order traversal to track the Kth element.
   - Stop the traversal once the Kth element is found, which will avoid unnecessary processing.

4. **Alternative Approaches**:  
   - Use a recursive or iterative approach for in-order traversal.
   - For very large trees, you can also use a stack to implement the traversal iteratively.

---

## Algorithm

**Pseudocode**:  
We will implement an in-order traversal and stop when the Kth smallest element is found.

```text
1. Initialize a counter `count` to 0.
2. Perform in-order traversal:
   - Visit the left child.
   - Visit the current node. Increment `count` and check if `count == k`. If yes, return the current node's value.
   - Visit the right child.
```
### alternative approach:
```text
   1. Initalize the list to store all the elements of tree during inorder traversal
   2. perform in order traversal 
     - Visit the left child.
        - Visit the current node. add the current node value to the result
        - Visit the right child.
   3. return the k - 1 th element from the res list.
```

**Time Complexity**:  
- The time complexity is \(O(N)\), where \(N\) is the number of nodes in the BST. In the worst case, we might need to visit all nodes.

**Space Complexity**:  
- The space complexity is \(O(H)\), where \(H\) is the height of the tree, due to the recursion stack (for recursive implementation).

---

## Solution Code (Implementation)

### Recursive Approach

```javascript
function kthSmallest(root, k) {
    let count = 0;
    let result = null;

    function inorder(node) {
        if (!node) return;

        inorder(node.left);  // Traverse left subtree
        count++;  // Visit node
        if (count === k) {
            result = node.val;  // Found the Kth smallest
            return;
        }
        inorder(node.right);  // Traverse right subtree
    }

    inorder(root);
    return result;
}
```

### Iterative Approach (Using a Stack)

```javascript
function kthSmallest(root, k) {
    const stack = [];
    let current = root;

    while (stack.length > 0 || current) {
        while (current) {
            stack.push(current);
            current = current.left;  // Go to the leftmost node
        }

        current = stack.pop();
        k--;  // Decrement k
        if (k === 0) {
            return current.val;  // Found the Kth smallest
        }

        current = current.right;  // Go to the right subtree
    }

    return -1;  // This should never be reached if k is valid
}
```
### java Iterative inorder (constant space) 

``` java
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
    public int kthSmallest(TreeNode root, int k) {
        if(root == null) return -1;

        Stack<TreeNode> st = new Stack<>();
        TreeNode current = root;
        while(current !=null || !st.isEmpty()){
            while(current != null){
                st.push(current);
                current = current.left;
            }
            current = st.pop();
            --k;
            if(k == 0) return current.val;
            current = current.right;
        }

        return -1;
    }
}
```
### java Recursive approach constant space

``` java
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
    int counter = 0;   
    public TreeNode inorder(TreeNode root, int k){
        if(root == null) return null;
        TreeNode left = inorder(root.left, k);
        if(left != null) return left;
        counter++;
        if(k == counter) return root;
        return inorder(root.right,k);
    }
    public int kthSmallest(TreeNode root, int k) {
        return inorder(root,k).val;
    }
}
```
### alternative approach Java using list (Recursive)

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
class Solution {
    public void inorder(TreeNode root, List<Integer> res){
        if(root == null) return;
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right,res);
    }
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        return res.get(k - 1);
    }
}
```
### itertive

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
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        if(root == null) return -1;
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> st = new Stack<>();
        TreeNode current = root;
        while(current !=null || !st.isEmpty()){
            while(current != null){
                st.push(current);
                current = current.left;
            }
            current = st.pop();
            res.add(current.val);
            current = current.right;
        }
        return res.get(k-1);
    }
}
```

---

## Walkthrough of the Solution

### Step 1: In-order Traversal
- Start with the root node and perform an in-order traversal. This ensures that the nodes are visited in ascending order.

### Step 2: Count the Nodes
- Maintain a `count` variable that tracks how many nodes have been visited so far.
- Once the Kth smallest node is encountered, return the value of that node.

### Step 3: Edge Case
- Ensure that the tree has at least `k` elements, which is guaranteed by the problem statement.

---

## Test Cases

| Input | Expected Output |
|-------|-----------------|
| `root = [3, 1, 4, null, 2], k = 1` | `1` |
| `root = [5, 3, 6, 2, 4, null, null, 1], k = 3` | `3` |
| `root = [1], k = 1` | `1` |
| `root = [2, 1, 3], k = 2` | `2` |

---

## Edge Cases

- **Empty Tree**: This should not occur as the problem guarantees at least `k` elements in the tree.
- **Single Node Tree**: Handle the case where the tree has only one node.
- **k = 1 or k = number of nodes in the tree**: Make sure the algorithm works even for the first or last smallest element.

---

This should be a comprehensive guide for solving the "Kth Smallest Element in a BST" problem. Let me know if you need further explanations or modifications!