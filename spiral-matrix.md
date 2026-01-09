# LeetCode 54: Spiral Matrix

## Problem Statement
Given an `m x n` matrix, return *all elements of the matrix in spiral order*.

### Example:
**Input:**
```java
matrix = [[1,2,3],[4,5,6],[7,8,9]]
```
**Output:**
```
[1,2,3,6,9,8,7,4,5]
```

## Constraints:
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

---

## Approach
We simulate the spiral traversal by using four boundaries:
- `top` – the first row not yet traversed
- `bottom` – the last row not yet traversed
- `left` – the first column not yet traversed
- `right` – the last column not yet traversed

We loop until the boundaries cross each other, and in each loop we:
1. Traverse the top row (left to right)
2. Traverse the rightmost column (top to bottom)
3. Traverse the bottom row (right to left) — only if `top <= bottom`
4. Traverse the leftmost column (bottom to top) — only if `left <= right`

We adjust the boundaries accordingly after each step.

---

## Algorithm Explanation (Step-by-Step)
1. Initialize an empty result list.
2. Set `top = 0`, `bottom = n-1`, `left = 0`, `right = m-1`.
3. While `top <= bottom` and `left <= right`:
   - Traverse from `left` to `right` on `top` row; increment `top`
   - Traverse from `top` to `bottom` on `right` column; decrement `right`
   - If `top <= bottom`: traverse from `right` to `left` on `bottom` row; decrement `bottom`
   - If `left <= right`: traverse from `bottom` to `top` on `left` column; increment `left`

---

## Dry Run Example
Input:
```
[ [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9] ]
```

Steps:
- Top Row → `1 2 3`
- Right Column → `6 9`
- Bottom Row → `8 7`
- Left Column → `4`
- Remaining center → `5`

**Result:** `[1,2,3,6,9,8,7,4,5]`

---

## Code (Java)
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int n = matrix.length;
        int m = matrix[0].length;
        int top = 0;
        int bottom = n - 1;
        int left = 0;
        int right = m - 1;

        while(top <= bottom && left <= right){
            // Top row
            for(int i = left; i <= right; i++){
                res.add(matrix[top][i]);
            }
            top++;

            // Right column
            for(int j = top; j <= bottom; j++){
                res.add(matrix[j][right]);
            }
            right--;

            // Bottom row (if still within bounds)
            if(top <= bottom){
                for(int i = right; i >= left; i--){
                    res.add(matrix[bottom][i]);
                }
                bottom--;
            }

            // Left column (if still within bounds)
            if(left <= right){
                for(int j = bottom; j >= top; j--){
                    res.add(matrix[j][left]);
                }
                left++;
            }
        }

        return res;
    }
}
```

---

## Time and Space Complexity
- **Time Complexity:** `O(m * n)` – each element is visited once.
- **Space Complexity:** `O(1)` – not including the output list.

---

## Summary
The spiral traversal uses boundary variables and simple for-loops to simulate the spiral path. Edge cases (e.g. single row/column) are handled via bounds checking (`top <= bottom`, `left <= right`). This algorithm is both clean and efficient for the matrix size constraints provided.

