
# 📘 LeetCode 207 – Course Schedule

## 🔗 Problem Link:
[https://leetcode.com/problems/course-schedule](https://leetcode.com/problems/course-schedule)

---

## 📝 Problem Statement

There are `numCourses` courses labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a, b]` indicates that you must take course `b` before course `a`.

Return `true` if it's possible to finish all courses. Otherwise, return `false`.

---

## 🔍 Example

```
Input: numCourses = 2, prerequisites = [[1, 0]]
Output: true

Input: numCourses = 2, prerequisites = [[1, 0], [0, 1]]
Output: false
```

---

## 🧠 Approach 1: DFS – Cycle Detection in Directed Graph

### ✅ Idea
Treat courses as nodes in a **directed graph** and detect **cycles** using DFS. If a cycle is found, return `false`.

---

### 🔄 Algorithm Steps – DFS

1. Build an adjacency list `adj` where `adj[a]` contains all prerequisites for course `a`.
2. For each course:
   - Run DFS from it.
   - Maintain a `visited` set for the **current DFS path**.
   - If we visit a course already in the current path, a cycle exists → return false.
   - After visiting all dependencies, mark the course as complete by setting its list to empty.
3. If DFS completes for all courses without detecting a cycle, return true.

---

### ⚙️ Java Code – DFS

```java
class Solution {
    public boolean dfs(int crs, Map<Integer, List<Integer>> adj, Set<Integer> visited) {
        if (visited.contains(crs)) return false;
        if (adj.get(crs).isEmpty()) return true;

        visited.add(crs);

        for (int pre : adj.get(crs)) {
            if (!dfs(pre, adj, visited)) return false;
        }

        visited.remove(crs);
        adj.put(crs, Collections.emptyList());  // Mark as completed
        return true;
    }

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        Map<Integer, List<Integer>> adj = new HashMap<>();
        for (int i = 0; i < numCourses; i++) {
            adj.put(i, new ArrayList<>());
        }

        for (int[] pre : prerequisites) {
            adj.get(pre[0]).add(pre[1]);  // course → prerequisite
        }

        Set<Integer> visited = new HashSet<>();

        for (int i = 0; i < numCourses; i++) {
            if (!dfs(i, adj, visited)) return false;
        }

        return true;
    }
}
```

---

### 🧮 Complexity – DFS

- **Time:** `O(V + E)`
- **Space:** `O(V + E)`

---

## 🧠 Approach 2: BFS – Kahn’s Algorithm (Topological Sort)

### ✅ Idea
Use **in-degree tracking** and perform **topological sorting** using BFS (Kahn’s algorithm). If all courses are processed, return `true`; otherwise, a cycle exists.

---

### 🔄 Algorithm Steps – BFS (Kahn's Algorithm)

1. Create an adjacency list `adj` and an array `inDegree[]` to count prerequisites for each course.
2. Initialize the queue with courses that have `inDegree = 0` (i.e., no prerequisites).
3. While the queue is not empty:
   - Pop a course from the queue and increment the completed course count.
   - For each dependent course, decrement its in-degree.
   - If in-degree becomes 0, add it to the queue.
4. After processing, if the completed count equals `numCourses`, return true. Else, a cycle exists → return false.

---

### ⚙️ Java Code – BFS (Kahn’s Algorithm)

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> adj = new ArrayList<>();
        int[] inDegree = new int[numCourses];

        // Initialize adjacency list
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }

        // Build graph and compute in-degrees
        for (int[] pre : prerequisites) {
            adj.get(pre[1]).add(pre[0]);
            inDegree[pre[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();

        // Add all courses with no prerequisites
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) queue.offer(i);
        }

        int completed = 0;

        while (!queue.isEmpty()) {
            int course = queue.poll();
            completed++;

            for (int next : adj.get(course)) {
                inDegree[next]--;
                if (inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }

        return completed == numCourses;
    }
}
```

---

### 🧮 Complexity – BFS (Kahn’s Algorithm)

| Metric      | Value            |
|-------------|------------------|
| **Time**    | `O(V + E)`       |
| **Space**   | `O(V + E)`       |

---

## ✅ Summary of Approaches

| Approach         | Detect Cycle | Uses Stack or Queue | Modifies Graph | Topo Sort |
|------------------|--------------|---------------------|----------------|-----------|
| DFS              | Yes          | Recursion Stack     | Yes            | Implicit  |
| BFS (Kahn’s)     | Yes          | Queue               | Yes (in-degree)| Explicit  |

