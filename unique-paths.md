## Explanation: Java Code for Unique Paths

```java
class Solution {
    public int uniquePaths(int m, int n) {
        long ans = 1L;
        for (int i = (m + n - 2), j = 1; i >= Math.max(m, n); i--, j++) {
            ans = (ans * i) / j;
        }
        return Long.valueOf(ans).intValue();
    }
}
```

This Java code calculates the number of unique paths from the top-left corner to the bottom-right corner of an `m x n` grid. The only allowed movements are **right** and **down**.

### Core Concept: Combinatorics
The problem is fundamentally a **combinatorics** problem. To reach the destination, you must take a total of `m + n - 2` steps. Of these steps, `m - 1` must be downward, and `n - 1` must be rightward. The number of unique paths is the number of ways to choose these downward (or rightward) steps, which is a **binomial coefficient**.

### Understanding Combinatorics
Combinatorics is a branch of mathematics dealing with counting and arrangement problems. In this case, we use the concept of permutations and combinations to determine the number of ways to arrange `m - 1` down moves and `n - 1` right moves in a total of `m + n - 2` steps. The formula used in this problem is:

\[
C(m+n-2, m-1) = \frac{(m+n-2)!}{(m-1)! (n-1)!}
\]

Instead of calculating factorials directly, which can lead to large numbers and overflow, we optimize the calculation using an iterative approach.

### Code Breakdown

#### `uniquePaths(int m, int n)`:  
- This is the method that takes the grid dimensions `m` (rows) and `n` (columns) as input.
- It returns the number of unique paths as an integer.

#### `long ans = 1L;`:  
- Initializes a variable `ans` to 1. This variable will store the calculated number of paths.
- It's declared as a `long` (note the `L`) to **prevent potential integer overflow** during intermediate calculations, as factorials can grow very quickly.

#### `for (int i = (m + n - 2), j = 1; i >= Math.max(m, n); i--, j++)`:  
- This is the **core loop** that calculates the binomial coefficient efficiently.
- `i` is initialized to `m + n - 2`, which is the total number of steps required to reach the destination.
- `j` is initialized to `1` and acts as a denominator in our calculation.
- The loop continues as long as `i` is greater than or equal to `Math.max(m, n)`. This is an **optimization**. We are calculating a combination, and this loop effectively **avoids calculating the full factorial**.
- `i--` decrements `i` in each iteration.
- `j++` increments `j` in each iteration.

#### `ans = (ans * i) / j;`:  
- This is the **key calculation**. In each iteration, it updates `ans` to calculate the binomial coefficient step-by-step. It's equivalent to the formula:  
  ```
  ans = ans * (i / j)
  ```
- This **iterative multiplication and division approach** is more efficient and **prevents overflow** compared to calculating full factorials.

#### `return Long.valueOf(ans).intValue();`:  
- After the loop finishes, `ans` holds the total number of unique paths (as a `long`).
- This line converts the `long` value of `ans` into an integer before returning the result.

