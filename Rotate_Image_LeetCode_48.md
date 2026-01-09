
# ✅ LeetCode Problem #48 - Rotate Image

## 🔸 Problem Statement

You are given an `n x n` 2D matrix representing an image. Rotate the image by 90 degrees (clockwise) **in-place**.

### Constraints
- `matrix.length == n`
- `matrix[i].length == n`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

### Example

**Input:**
```
matrix = [
 [1, 2, 3],
 [4, 5, 6],
 [7, 8, 9]
]
```

**Output:**
```
[
 [7, 4, 1],
 [8, 5, 2],
 [9, 6, 3]
]
```

---

## 🧠 Intuition

To rotate the image **in-place**, two common approaches work well:
- **Transpose + Reverse Rows**
- **Vertical Reverse + Transpose**

---

## ✅ Approach 1: Vertical Reverse + Transpose

### 🔹 Steps

1. Reverse the matrix vertically (swap top and bottom rows).
2. Transpose the matrix by swapping `matrix[i][j]` with `matrix[j][i]`.

### 🔹 Python Code

```python
def rotate(matrix):
    n = len(matrix)
    matrix.reverse()
    for i in range(n):
        for j in range(i+1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

### 🔹 Java Code

```java
public class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++) {
            int[] temp = matrix[i];
            matrix[i] = matrix[n - 1 - i];
            matrix[n - 1 - i] = temp;
        }
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}
```

```java
class Solution {
    public void rotate(int[][] matrix) {
        int edgeLength = matrix.length;

        int top = 0;
        int bottom = edgeLength - 1;

        // vertical reversal 
        // swap bottom rows and vertical rows
        // swap until we met middle row
        while(top < bottom){
            for(int col = 0;col < edgeLength;col++){
                int temp = matrix[top][col];
                matrix[top][col] = matrix[bottom][col];
                matrix[bottom][col] = temp;
            }
            top++;
            bottom--;
        }

        // Transposal matrix 
        // swap [row][col] with [col][row]
        for(int row = 0;row < edgeLength;row++){
            // we define next value of the row in col in previous
            // operation we performed previous row transposal
            for(int col = row + 1;col<edgeLength;col++){
                int temp = matrix[row][col];
                matrix[row][col] = matrix[col][row];
                matrix[col][row] = temp;
            }
        }
    }
}
```

---

## ✅ Approach 2: Transpose + Reverse Each Row

### 🔹 Steps

1. Transpose the matrix.
2. Reverse each row individually.

### 🔹 Python Code

```python
def rotate(matrix):
    n = len(matrix)
    for i in range(n):
        for j in range(i+1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    for row in matrix:
        row.reverse()
```

### 🔹 Java Code

```java
public class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

```java
class Solution {
    public void rotate(int[][] matrix) {
        // third algorithm 
        // transposal the array
        // swap [row][col] with [col][row]

        for(int row = 0;row < matrix.length;row++){
           for(int col = 0; col <= row;col++){
              int temp = matrix[row][col];
              matrix[row][col] = matrix[col][row];
              matrix[col][row] = temp;
           }
        }

        for(int i = 0;i < matrix.length;i++){
            // reverse the row
            int start = 0;
            int end = matrix[i].length - 1;
            while(start < end){
                int temp = matrix[i][start];
                matrix[i][start] = matrix[i][end];
                matrix[i][end] = temp;
                start++;
                end--;
            }
        }
    }
}
```

---

## 🧮 Time & Space Complexity

| Operation           | Time Complexity | Space Complexity |
|---------------------|------------------|-------------------|
| Vertical Reverse    | O(n)             | O(1)              |
| Transpose           | O(n²)            | O(1)              |
| Reverse Each Row    | O(n²)            | O(1)              |
| **Overall**         | **O(n²)**        | **O(1)**          |

---

## 🎯 Visual: 3x3 Rotation

**Before:**
```
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
```

**After 90° Clockwise Rotation:**
```
[7, 4, 1]
[8, 5, 2]
[9, 6, 3]
```
