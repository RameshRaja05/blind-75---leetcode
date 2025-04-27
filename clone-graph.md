Great! You've implemented the **BFS approach** for the **Clone Graph** problem in Java. Below is the updated markdown documentation with this implementation added, including a breakdown of the BFS-based approach:

---

```markdown
# Clone Graph - LeetCode Problem

## Problem Statement

Given a reference of a node in a **connected undirected graph**, return a **deep copy (clone)** of the graph.

Each node contains:
- An integer value `val`
- A list of neighbors `List<Node> neighbors`

---

### Java Node Definition

```java
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }

    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }

    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
```

---

## Input Format

- Each node’s value is 1-indexed and corresponds to its position in the adjacency list.
- The given node is always the node with `val = 1`.

---

## Examples

### Example 1

**Input**:  
`adjList = [[2,4],[1,3],[2,4],[1,3]]`  
**Output**:  
`[[2,4],[1,3],[2,4],[1,3]]`

### Example 2

**Input**: `adjList = [[]]`  
**Output**: `[[]]`  

### Example 3

**Input**: `adjList = []`  
**Output**: `[]`  

---

## Constraints

- Number of nodes: [0, 100]
- 1 <= Node.val <= 100
- Node values are unique
- No self-loops or repeated edges
- Graph is connected and can be fully traversed from node 1

---

## Approach: BFS (Breadth-First Search)

We use a **queue** to perform a level-order traversal (BFS) and a **hash map** to keep track of already copied nodes.

### Step-by-Step

1. If the input node is `null`, return `null`.
2. Create a map `copied` to store original `val → cloned Node`.
3. Create the first cloned node and enqueue the original.
4. While the queue is not empty:
   - Dequeue the current node.
   - For each neighbor:
     - If it’s not cloned yet, clone and enqueue it.
     - Add the cloned neighbor to the current clone’s neighbor list.
5. Return the initially cloned node.

---

## Java Code (BFS)

```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }

        Map<Integer, Node> copied = new HashMap<>();
        Queue<Node> queue = new ArrayDeque<>();

        Node cloneNode = new Node(node.val);
        copied.put(node.val, cloneNode);
        queue.add(node);

        while (!queue.isEmpty()) {
            Node curNode = queue.poll();
            Node clonedCurNode = copied.get(curNode.val);

            for (Node nei : curNode.neighbors) {
                if (!copied.containsKey(nei.val)) {
                    copied.put(nei.val, new Node(nei.val));
                    queue.add(nei);
                }
                clonedCurNode.neighbors.add(copied.get(nei.val));
            }
        }

        return cloneNode;
    }
}
```
## Java Code (DFS)

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public HashMap<Node,Node> oldToNew = new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node == null){
            return null;
        }

        Stack<Node> st = new Stack<>();
        st.push(node);
        oldToNew.put(node, new Node(node.val));

        while(!st.empty()){

           Node n = st.pop();
           Node cloneNode = oldToNew.get(n);

           for(Node nei:n.neighbors){
                if(!oldToNew.containsKey(nei)){
                    Node cloneNeighbor = new Node(nei.val);
                    oldToNew.put(nei,cloneNeighbor);
                    st.push(nei);
                }

               cloneNode.neighbors.add(oldToNew.get(nei));
           }      
        }

        return oldToNew.get(node);
    }
}
```

## Java Code (Recursion)

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    HashMap<Node,Node> oldToNew = new HashMap<>();

    public Node clone(Node node){
       // if node already exist return the node
       if(oldToNew.containsKey(node)){
          return oldToNew.get(node);
       }

       // create a new node
       Node cloneNode = new Node(node.val);
       oldToNew.put(node, cloneNode);

       for(Node nei:node.neighbors){
          cloneNode.neighbors.add(clone(nei));
       }

       return cloneNode;
    }

    public Node cloneGraph(Node node) {
        if(node == null){
            return null;
        }
        
        return clone(node);
    }
}
```
---

## Time and Space Complexity

- **Time Complexity**: O(N + E), where:
  - N is the number of nodes
  - E is the number of edges
- **Space Complexity**: O(N) for the map and queue

---

## Comparison: DFS vs BFS

| Criteria      | DFS                         | BFS                          |
|---------------|------------------------------|-------------------------------|
| Data Structure | Recursion + HashMap          | Queue + HashMap               |
| Order          | Depth-first                  | Level-order (Breadth-first)   |
| Use Case       | Smaller graphs, simple logic | Avoid recursion depth limits  |

---

```

Let me know if you want both **BFS and DFS versions** included in one file, or if you'd like to generate a `.md` file for download!