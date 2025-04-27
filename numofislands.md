Here's a detailed markdown documentation of the LeetCode problem **Number of Islands** along with the approach and solution:

---

### `📘 Problem: Number of Islands`

**LeetCode ID:** 200  
**Difficulty:** Medium  
**Topic:** Depth-First Search (DFS), Breadth-First Search (BFS), Union Find

---

#### 🧾 Problem Description

You are given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water). Return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands **horizontally** or **vertically**. You may assume all four edges of the grid are surrounded by water.

---

#### 📥 Input

- `grid`: 2D list of strings (`'0'` or `'1'`), size `m x n`
- `1 <= m, n <= 300`

---

#### 📤 Output

- Integer: total number of **distinct islands**

---

#### 📚 Examples

**Example 1:**
```txt
Input: 
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**
```txt
Input: 
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

---

### 🧠 Approach

We will use **Depth-First Search (DFS)** to solve this problem.

---

### 🔍 Step-by-Step Walkthrough

#### Step 1: Loop through the grid
Iterate through each cell of the matrix. Every time we encounter a `'1'`, it means we found new land (i.e., potentially a new island).

#### Step 2: DFS from that cell
Use DFS to explore all `'1'`s connected to that cell (up, down, left, right). During the exploration, we **mark visited cells as `'0'`** to avoid revisiting them.

#### Step 3: Count the islands
Each time a DFS is triggered, we increment the island counter, since it means we have found a new disconnected landmass.

---

### 🧮 Time & Space Complexity

| Type              | Complexity       |
|-------------------|------------------|
| Time Complexity   | `O(m * n)`        |
| Space Complexity  | `O(m * n)` (stack space for DFS in worst case) |

---

### 🧑‍💻 Code (Python)

```python
def numIslands(grid):
    if not grid:
        return 0

    def dfs(r, c):
        # Check bounds and if current cell is land
        if r < 0 or c < 0 or r >= len(grid) or c >= len(grid[0]) or grid[r][c] == '0':
            return
        # Mark cell as visited
        grid[r][c] = '0'
        # Visit all 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    count = 0
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                dfs(r, c)
                count += 1
    return count
```
### 🧑‍💻 Code (Java) BFS
``` Java
class Solution {
    public void bfs(char[][] grid, int row, int col, Set<String> visited, int rows, int cols){
        Queue<int[]> q = new LinkedList<>();
        visited.add(row+","+col);
        q.add(new int[]{row,col});

        int[][] directions = {{-1,0},{1,0},{0,-1},{0,1}};
        while(!q.isEmpty()){
            int[] position = q.poll();
            int r = position[0], c = position[1];

            for(int[] direction:directions){
                int nr = r + direction[0];
                int nc = c + direction[1];

                if(nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == '1' && !visited.contains(nr+","+nc)){
                    q.add(new int[]{nr,nc});
                    visited.add(nr+","+nc);
                }
            }
        }
    }
    public int numIslands(char[][] grid) {
        int numOfIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;

        Set<String> visited = new HashSet<>();

        for(int i = 0;i < rows;i++){
            for(int j = 0;j < cols;j++){
                if(grid[i][j] == '1' && !visited.contains(i+","+j)){
                    numOfIslands++;
                    bfs(grid, i, j, visited, rows, cols);
                }
            }
        }

        return numOfIslands;
    }
}

```


---

### ✅ Dry Run on Example 2

```txt
Grid:
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1

Pass 1: Encounter (0,0) → DFS marks all connected land → count = 1
Pass 2: Encounter (2,2) → DFS marks single land → count = 2
Pass 3: Encounter (3,3) → DFS marks (3,3) and (3,4) → count = 3
Final Count = 3
```

---

### 🌀 Alternate Approaches

- **Breadth-First Search (BFS)**: Use a queue instead of recursion.
- **Union-Find (Disjoint Set)**: Treat each land cell as a node in a union-find structure.

---

Let me know if you'd like this saved into an actual `.md` file!