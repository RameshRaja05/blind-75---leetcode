# LeetCode 417. Pacific Atlantic Water Flow

## 🧩 Problem Description
Given an `m x n` matrix of non-negative integers representing the height of each cell in a continent, the "Pacific Ocean" touches the left and top edges of the matrix and the "Atlantic Ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with **equal or lower height**.

Return a list of grid coordinates where water can flow to **both the Pacific and Atlantic oceans**.

### Example:
```text
Input: heights = [
  [1, 2, 2, 3, 5],
  [3, 2, 3, 4, 4],
  [2, 4, 5, 3, 1],
  [6, 7, 1, 4, 5],
  [5, 1, 1, 2, 4]
]

Output: [
  [0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]
]
```

### Constraints:
- `m == heights.length`
- `n == heights[i].length`
- `1 <= m, n <= 200`
- `0 <= heights[i][j] <= 10^5`

---

## 🧠 Approach Overview
To determine which cells can flow into both the Pacific and Atlantic Oceans:

1. **Reverse Thinking:** Instead of simulating water flow from every cell to the oceans, simulate from **the oceans inward**.
2. **DFS or BFS from Ocean Borders:**
   - Start from all Pacific-bordering cells and mark all reachable cells.
   - Do the same from all Atlantic-bordering cells.
   - The intersection of both reachable cell sets gives the final answer.

---

## 🔍 Step-by-Step Walkthrough

### 📍 Step 1: Initialize
- Define two sets (or matrices) to track cells reachable from Pacific and Atlantic.

### 📍 Step 2: DFS Traversal from Border Cells
- Perform DFS (or BFS) from all cells on the top and left for Pacific.
- Perform DFS (or BFS) from all cells on the bottom and right for Atlantic.
- In DFS, move to neighboring cells only if the neighbor's height is **>= current height**.

### 🌊 Visual Matrix Borders
```text
Pacific ~ Top & Left
Atlantic ~ Bottom & Right

Example:
P P P P P
P       A
P       A
P       A
P A A A A
```

### 📍 Step 3: Result Intersection
- Return all cells that are present in **both** the Pacific-reachable and Atlantic-reachable sets.

---

## 🧮 Algorithm Details

### DFS Code (Python):
```python
from typing import List

class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        if not heights:
            return []

        rows, cols = len(heights), len(heights[0])
        pacific = set()
        atlantic = set()

        def dfs(r, c, visited, prev_height):
            if (
                r < 0 or c < 0 or r >= rows or c >= cols
                or (r, c) in visited
                or heights[r][c] < prev_height
            ):
                return

            visited.add((r, c))
            for dr, dc in [(1,0), (-1,0), (0,1), (0,-1)]:
                dfs(r + dr, c + dc, visited, heights[r][c])

        for c in range(cols):
            dfs(0, c, pacific, heights[0][c])  # Top
            dfs(rows - 1, c, atlantic, heights[rows - 1][c])  # Bottom

        for r in range(rows):
            dfs(r, 0, pacific, heights[r][0])  # Left
            dfs(r, cols - 1, atlantic, heights[r][cols - 1])  # Right

        return list(pacific & atlantic)
```

### BFS Version (Python):
```python
from collections import deque

class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        rows, cols = len(heights), len(heights[0])
        pacific = set()
        atlantic = set()

        def bfs(queue, visited):
            while queue:
                r, c = queue.popleft()
                visited.add((r, c))
                for dr, dc in [(1,0), (-1,0), (0,1), (0,-1)]:
                    nr, nc = r + dr, c + dc
                    if (
                        0 <= nr < rows and 0 <= nc < cols
                        and (nr, nc) not in visited
                        and heights[nr][nc] >= heights[r][c]
                    ):
                        queue.append((nr, nc))

        pac_queue = deque()
        atl_queue = deque()

        for c in range(cols):
            pac_queue.append((0, c))
            atl_queue.append((rows - 1, c))
        for r in range(rows):
            pac_queue.append((r, 0))
            atl_queue.append((r, cols - 1))

        bfs(pac_queue, pacific)
        bfs(atl_queue, atlantic)

        return list(pacific & atlantic)
```

### DFS Code (Java):
```java
class Solution {
    private int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    private int rows, cols;

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        rows = heights.length;
        cols = heights[0].length;
        boolean[][] pacific = new boolean[rows][cols];
        boolean[][] atlantic = new boolean[rows][cols];

        for (int c = 0; c < cols; c++) {
            dfs(heights, 0, c, pacific, heights[0][c]);
            dfs(heights, rows - 1, c, atlantic, heights[rows - 1][c]);
        }

        for (int r = 0; r < rows; r++) {
            dfs(heights, r, 0, pacific, heights[r][0]);
            dfs(heights, r, cols - 1, atlantic, heights[r][cols - 1]);
        }

        List<List<Integer>> result = new ArrayList<>();
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (pacific[r][c] && atlantic[r][c]) {
                    result.add(Arrays.asList(r, c));
                }
            }
        }
        return result;
    }

    private void dfs(int[][] heights, int r, int c, boolean[][] visited, int prevHeight) {
        if (r < 0 || c < 0 || r >= rows || c >= cols || visited[r][c] || heights[r][c] < prevHeight) {
            return;
        }
        visited[r][c] = true;
        for (int[] dir : directions) {
            dfs(heights, r + dir[0], c + dir[1], visited, heights[r][c]);
        }
    }
}
```

---

## ⏱ Time and Space Complexity

### DFS Approach:
- **Time:** `O(m * n)` — each cell is visited at most twice (once for each ocean).
- **Space:** `O(m * n)` — for the recursion stack and visited sets.

### BFS Approach:
- **Time:** `O(m * n)` — each cell is processed once per ocean.
- **Space:** `O(m * n)` — queue and visited sets.

---

## ✅ Final Notes
- Simulating from the ocean inward avoids repeated traversal from every cell.
- Use DFS for elegant recursion and BFS for level-order/queue based traversal.
- Works well even for max constraints `200 x 200`.

