
# 🧩 LeetCode 297: Serialize and Deserialize Binary Tree

## 📝 Problem Statement

Design an algorithm to serialize and deserialize a binary tree. Serialization is the process of converting a tree to a string so that it can be stored or transmitted and then later reconstructed.

You must design an algorithm that is both stateless and efficient.

---

# 💡 Approach 1: DFS (Preorder Traversal)

We use **Depth-First Search (DFS)** — specifically **Preorder Traversal (Root, Left, Right)** — to serialize and deserialize the tree.

- A **null** node is represented with a special marker `"N"`.
- A comma (`,`) is used as a delimiter between nodes.

## ⚙️ Algorithm

### 🔹 Serialization

1. Traverse the tree using preorder.
2. Append each value to a `StringBuilder`.
3. Use `"N"` for null nodes.

### 🔹 Deserialization

1. Split the serialized string by comma.
2. Use a recursive helper function to rebuild the tree from the preorder list.

## 🔍 Example

Input Tree:
```
        1
       / \
      2   3
         / \
        4   5
```

Serialized Output:
```
"1,2,N,N,3,4,N,N,5,N,N,"
```

## 💻 Code (DFS Java)

```java
public class Codec {
    private int i = 0;

    public void serializeTree(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("N,");
            return;
        }
        sb.append(node.val).append(",");
        serializeTree(node.left, sb);
        serializeTree(node.right, sb);
    }

    public TreeNode buildTree(String[] nodes) {
        if (nodes[i].equals("N")) {
            i++;
            return null;
        }
        TreeNode node = new TreeNode(Integer.parseInt(nodes[i++]));
        node.left = buildTree(nodes);
        node.right = buildTree(nodes);
        return node;
    }

    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeTree(root, sb);
        return sb.toString();
    }

    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        return buildTree(nodes);
    }
}
```

---

# 💡 Approach 2: BFS (Level Order Traversal)

We use **Breadth-First Search (BFS)** or **Level Order Traversal** to serialize and deserialize the tree.

- Each node's value is followed by its children in order.
- `"null"` is used for missing nodes.

## ⚙️ Algorithm

### 🔹 Serialization

1. Use a queue and level-order traversal.
2. Append each node value or `"null"` to the result string.

### 🔹 Deserialization

1. Split the string by commas.
2. Use a queue to attach children to each parent node in level order.

## 🔍 Example

Input Tree:
```
        1
       / \
      2   3
         / \
        4   5
```

Serialized Output:
```
"1,2,3,null,null,4,5"
```

## 💻 Code (BFS Java)

```java
public class Codec {
    public String serialize(TreeNode root) {
        if (root == null) return "";

        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode curr = queue.poll();
            if (curr == null) {
                sb.append("null,");
                continue;
            }

            sb.append(curr.val).append(",");
            queue.offer(curr.left);
            queue.offer(curr.right);
        }

        return sb.toString().replaceAll(",+$", "");
    }

    public TreeNode deserialize(String data) {
        if (data == null || data.isEmpty()) return null;

        String[] values = data.split(",");
        TreeNode root = new TreeNode(Integer.parseInt(values[0]));

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        int i = 1;
        while (!queue.isEmpty() && i < values.length) {
            TreeNode parent = queue.poll();

            if (!values[i].equals("null")) {
                parent.left = new TreeNode(Integer.parseInt(values[i]));
                queue.offer(parent.left);
            }
            i++;

            if (i < values.length && !values[i].equals("null")) {
                parent.right = new TreeNode(Integer.parseInt(values[i]));
                queue.offer(parent.right);
            }
            i++;
        }

        return root;
    }
}
```

---

## 🧪 Time and Space Complexity

| Operation     | Time Complexity | Space Complexity |
|---------------|-----------------|------------------|
| Serialize     | O(n)            | O(n)             |
| Deserialize   | O(n)            | O(n)             |

Where `n` is the number of nodes in the tree.

---

## ✅ Summary

- **DFS (Preorder)** is concise and elegant, useful for recursive reconstruction.
- **BFS (Level Order)** maintains complete structure level-wise.
- Both methods are valid and efficient depending on the use case.
