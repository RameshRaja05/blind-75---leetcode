Here is the **Markdown version** of the documentation for the **Number of Connected Components in an Undirected Graph** problem:

---

# 🧩 Number of Connected Components in an Undirected Graph

### ✅ Problem Statement

Given an undirected graph with `n` nodes labeled from `0` to `n - 1` and a list of undirected edges, return the **number of connected components** in the graph.

---

### 🔍 Intuition

A **connected component** is a subgraph in which every pair of vertices is connected by a path. Using **DFS** (or **BFS**), we can explore all nodes in a component starting from an unvisited node. Each time we begin a new traversal from an unvisited node, we’ve found a new component.

---

### 🧠 Approach (DFS)

1. Build an adjacency list from the edges.
2. Initialize a visited array to track visited nodes.
3. For each node from `0` to `n - 1`, if it hasn't been visited:

   * Perform DFS from that node.
   * Increment the component count.

---

### 🧪 Example

**Input:**

```txt
n = 5
edges = [[0,1],[1,2],[3,4]]
```

**Output:**

```txt
2
```

**Explanation:**

* Component 1: 0–1–2
* Component 2: 3–4

---

### 💻 DFS-Based Java Implementation

```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        // Build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]); // because it's undirected
        }

        boolean[] visited = new boolean[n];
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited);
                count++;
            }
        }

        return count;
    }

    private void dfs(int node, List<List<Integer>> adj, boolean[] visited) {
        visited[node] = true;
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor, adj, visited);
            }
        }
    }
}
```

---

### 🧮 Dry Run

For `n = 5`, `edges = [[0,1],[1,2],[3,4]]`:

* DFS starts at `0`: visits `0`, `1`, `2` → component count = 1
* Next unvisited node is `3`: visits `3`, `4` → component count = 2

---

### ⏱️ Time and Space Complexity

| Metric | Value                                     |
| ------ | ----------------------------------------- |
| Time   | O(N + E) — N nodes, E edges               |
| Space  | O(N + E) — adjacency list + visited array |

---

Let me know if you'd like a BFS version or want this included in a LeetCode-style notes folder!

## union - find

# Number of Connected Components in an Undirected Graph (LeetCode 323)

---

## 🔍 Problem Recap

Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges, return the number of connected components in the graph.

---

## ⚖️ High-Level Idea

1. Initially, each node is its own component.
2. When an edge connects two nodes in different components, we **merge** those components.
3. The total number of merges reduces the total number of components.

Use **Union-Find (Disjoint Set Union, DSU)** to efficiently manage the merging of components.

---

## ⚙️ Union-Find Data Structure

| Array       | Purpose                             | Initialization  |
| ----------- | ----------------------------------- | --------------- |
| `parent[i]` | parent pointer for node `i`         | `parent[i] = i` |
| `rank[i]`   | tree height or size (for balancing) | `rank[i] = 0`   |

### Operations

* **Find(x)**: Recursively find the root of `x`, applying path compression.
* **Union(x, y)**: Connect the roots of `x` and `y` using rank to keep tree shallow.

---

## ✅ Algorithm Steps

```text
1. Initialize parent and rank arrays.
2. Set components = n.
3. For each edge (u, v):
    a. Find roots of u and v.
    b. If roots differ:
        i. Union the sets.
        ii. components -= 1
4. Return components
```

---

## ⏱️ Time and Space Complexity

* **Time:** O(n + e \* α(n)) where α is the inverse Ackermann function (nearly constant).
* **Space:** O(n)

---

## 🔄 Python Implementation

```python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        parent = list(range(n))
        rank = [0] * n

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            rx, ry = find(x), find(y)
            if rx == ry:
                return False
            if rank[rx] < rank[ry]:
                parent[rx] = ry
            elif rank[rx] > rank[ry]:
                parent[ry] = rx
            else:
                parent[ry] = rx
                rank[rx] += 1
            return True

        components = n
        for u, v in edges:
            if union(u, v):
                components -= 1
        return components
```

---

## 📂 Java Implementation

```java
class DSU{
   int[] parent;
   int[] rank;

   public DSU(int n){
        parent = new int[n];
        rank = new int[n];
        for(int i = 0;i < n;i++){
            parent[i] = i;
            rank[i] = i;
        }
   }
   // find common parent of the node
   public int find(int node){
       int cur = node;
       while(cur != parent[cur]){
          // optimization line
          parent[cur] = parent[parent[cur]];
          cur = parent[cur];
       }

       return cur;
   }

   // merge two nodes
   public int union(int n1, int n2){
        int p1 = find(n1);
        int p2 = find(n2);
        // handling duplicate edges
        if(p1 == p2){
            return 0;
        }
        // merge small rank node to the high rank node
        if(rank[p1] > rank[p2]){
            parent[p2] = p1;
            rank[p1]++;
        }else{
            parent[p1] = p2;
            rank[p2]++;
        }

        return 1;
   }
}

class Solution {
    public int countComponents(int n, int[][] edges) {
        DSU dsu = new DSU(n);
        int res = n;

        for(int[] edge:edges){
            res -= dsu.union(edge[0], edge[1]);
        }

        return res;
    }
}

```

---

## ⚠️ Edge Cases

* **Duplicate edges**: union returns `false`; no change.
* **Self-loops**: ignored.
* **Isolated nodes**: handled by initializing `components = n`.

---

## 📊 Visualization (Optional)

```
Before any edges:
0   1   2   3   4    => 5 components

Add edge (0, 1): merge 0 and 1
0-1  2   3   4       => 4 components

Add edge (1, 2): merge 0-1 with 2
0-1-2   3   4       => 3 components

...
```

---

## 🤝 Summary

* Use Union-Find for efficient merging of sets.
* Each successful union reduces component count.
* Final count = number of connected components.

---

Let me know if you'd like to extend this with BFS/DFS versions or add visual diagrams!
