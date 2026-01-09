# LeetCode 261: Graph Valid Tree

## Problem Statement
You are given `n` nodes labeled from `0` to `n - 1` and a list of `edges` where each `edges[i] = [ai, bi]` represents an undirected edge between nodes `ai` and `bi`.

Return `true` *if the edges make up a valid tree*, and `false` otherwise.

### Example:
**Input:**
```java
n = 5
edges = [[0,1],[0,2],[0,3],[1,4]]
```
**Output:** `true`

### Constraints:
- `1 <= n <= 2000`
- `0 <= edges.length <= n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no duplicate edges.

---

## Approach: Depth-First Search (DFS)
To determine if the graph is a valid tree, we need to ensure:

1. **Acyclic:** No cycles exist in the graph.
2. **Connected:** All nodes are part of a single connected component.

### Observations:
- A tree with `n` nodes must have **exactly `n - 1` edges**.
- We build an adjacency list and perform DFS to detect cycles.
- During DFS, we track visited nodes and make sure we don’t revisit nodes (except the immediate parent).

---

## Algorithm Explanation
1. **Build Adjacency List:** Create an undirected graph.
2. **DFS Traversal:**
   - Start DFS from node `0`.
   - If a visited node is encountered again (excluding its parent), a cycle is detected.
   - Mark each node as visited.
3. After DFS completes, ensure that **all nodes are visited** (to ensure connectivity).

If both conditions hold — no cycles and all nodes are visited — return `true`.

---

## Dry Run Example
**Input:**
```java
n = 5
edges = [[0,1],[0,2],[0,3],[1,4]]
```

DFS from node `0`:
- Visit 0 → 1 → 4
- Backtrack and go 0 → 2
- Backtrack and go 0 → 3
- All nodes visited, no cycles found.

✅ Valid Tree

---

## Code (Java)
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();

        // Initialize adjacency list
        for(int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Build undirected graph
        for(int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }

        // Run DFS starting from node 0
        if(!dfs(-1, 0, visited, adj)) {
            return false; // Cycle detected
        }

        // Check connectivity
        return visited.size() == n;
    }

    public boolean dfs(int parent, int current, Set<Integer> visited, List<List<Integer>> adj) {
        if(visited.contains(current)) {
            return false; // Cycle detected
        }

        visited.add(current);

        for(int nei : adj.get(current)) {
            if(nei == parent) continue; // Skip edge to parent

            if(!dfs(current, nei, visited, adj)) {
                return false;
            }
        }

        return true;
    }
}
```

---

## Time and Space Complexity
- **Time Complexity:** `O(N + E)` where `N` is number of nodes and `E` is number of edges.
- **Space Complexity:** `O(N + E)` for adjacency list and visited set.

---

## Summary
This DFS approach ensures that the graph is both connected and acyclic. The base check `edges.length != n - 1` (which you could add) is also a quick early-out check for invalid trees.

