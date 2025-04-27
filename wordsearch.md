Awesome — you've implemented an optimized **backtracking solution with search pruning** for the **Word Search** problem in Java. Here's a clean and detailed **Markdown documentation** of the problem, your approach, and your Java solution:

---

## 📘 Problem: Word Search

**LeetCode ID:** 79  
**Difficulty:** Medium  
**Tags:** Backtracking, DFS, Matrix Traversal, Search Pruning

---

### 🧾 Problem Statement

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are **horizontally or vertically** neighboring. The same letter cell may **not** be used more than once.

---

### 📥 Input

- `board`: 2D character matrix of size `m x n`
- `word`: String of length up to 15

### 📤 Output

- `true` if the word exists in the grid, `false` otherwise

---

### 📚 Examples

#### Example 1

```txt
Input: 
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
], 
word = "ABCCED"

Output: true
```

#### Example 2

```txt
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]
word = "SEE"
Output: true
```

#### Example 3

```txt
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]
word = "ABCB"
Output: false
```

---

### 🧠 Approach

We use **backtracking + DFS** to explore the board and check if the word exists by following these steps:

---

### 🔍 Step-by-Step Walkthrough

1. **Search Start**  
   Loop through each cell in the grid. If the character at that cell matches the first character of the word, trigger a recursive DFS.

2. **Backtracking Logic**  
   At each step:
   - Check for out-of-bound or already visited cell.
   - If the current character matches, recursively check in all 4 directions (up, down, left, right).
   - Use a `Set` to track visited positions in `"r,c"` format to avoid revisiting.

3. **Pruning Optimization**  
   - Reverse the word if the last character is rarer than the first one to fail early (search pruning).
   - Use a `HashMap<Character, Integer>` to count frequency of characters in the word.

---

### 🧮 Time & Space Complexity

| Type              | Complexity         |
|------------------|--------------------|
| Time Complexity   | `O(m * n * 4^L)`   |
| Space Complexity  | `O(L)` recursion stack, `O(L)` visited set |

Where `m x n` is the grid size and `L` is the length of the word.

---

### 🧑‍💻 Java Code

```java
class Solution {
    public boolean backtrack(char[][] board, int r, int c, int k, Set<String> visited, String word, int rows, int cols){
        if(k == word.length()){
            return true;
        }

        if(r < 0 || r >= rows || c < 0 || c >= cols || 
           visited.contains(r + "," + c) || 
           board[r][c] != word.charAt(k)){
            return false;
        }

        visited.add(r + "," + c);

        boolean res = backtrack(board, r - 1, c, k + 1, visited, word, rows, cols) ||
                      backtrack(board, r + 1, c, k + 1, visited, word, rows, cols) ||
                      backtrack(board, r, c + 1, k + 1, visited, word, rows, cols) ||
                      backtrack(board, r, c - 1, k + 1, visited, word, rows, cols);

        visited.remove(r + "," + c);

        return res;
    }

    public boolean exist(char[][] board, String word) {
        int rows = board.length;
        int cols = board[0].length;
        HashSet<String> visited = new HashSet<>();

        // Frequency map for characters in the word
        Map<Character, Integer> charCount = new HashMap<>();
        for(char c : word.toCharArray()){
            charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        }

        // Word reversal optimization
        if(charCount.getOrDefault(word.charAt(0), 0) > 
           charCount.getOrDefault(word.charAt(word.length() - 1), 0)){
            word = new StringBuilder(word).reverse().toString();
        }

        for(int r = 0; r < rows; r++){
            for(int c = 0; c < cols; c++){
                if(backtrack(board, r, c, 0, visited, word, rows, cols)){
                    return true;
                }
            }
        }

        return false;
    }
}
```

---

### 🧪 Dry Run (word = "ABCCED")

```txt
Start at (0,0): 'A' → ✔
Right to (0,1): 'B' → ✔
Right to (0,2): 'C' → ✔
Down to (1,2): 'C' → ✔
Down to (2,2): 'E' → ✔
Left to (2,1): 'D' → ✔
→ All characters matched → return true ✅
```

---

### 🧰 Additional Enhancements (Optional)

- Use a `boolean[][] visited` array instead of a `Set` for faster performance.
- Avoid using `r+","+c` strings — integer-based keys or visited matrix is more efficient.

---

Let me know if you'd like this exported as a `.md` file or need this documented for multiple solutions (like BFS or Trie).

Here’s a detailed markdown documentation for the LeetCode problem **Word Search**, including the approach, dry run, and optimized solution with pruning.

---

### `📘 Problem: Word Search`

**LeetCode ID:** 79  
**Difficulty:** Medium  
**Topic:** Backtracking, DFS, Recursion

---

#### 🧾 Problem Description

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are **horizontally or vertically** neighboring. The same letter cell may **not be used more than once**.

---

### 📥 Input

- `board`: 2D character grid
- `word`: a string to search in the grid

---

### 📤 Output

- Boolean: `True` if word exists in the grid, else `False`

---

### 📚 Examples

**Example 1:**

```txt
Input: 
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
], 
word = "ABCCED"

Output: true
```

**Example 2:**

```txt
Input: 
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
], 
word = "SEE"

Output: true
```

**Example 3:**

```txt
Input: 
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
], 
word = "ABCB"

Output: false
```

---

### 🧠 Approach

We use **backtracking (DFS)** to explore each cell recursively to match the given word.

---

### 🔍 Step-by-Step Walkthrough

#### Step 1: Loop through each cell in the grid  
Check each cell. If the cell matches the first character of the word, initiate a DFS search from that cell.

#### Step 2: DFS Search  
- From each starting point, recursively check all 4 adjacent directions (up, down, left, right).
- Maintain a visited state by **temporarily marking cells**.
- Restore the cell after the search (backtracking step).

#### Step 3: Return true if the word is found, else false

---

### 🧮 Time & Space Complexity

| Type              | Complexity       |
|-------------------|------------------|
| Time Complexity   | `O(m * n * 4^L)` where `L = len(word)` |
| Space Complexity  | `O(L)` recursion stack |

---

### 🧑‍💻 Code (Python)

```python
def exist(board, word):
    rows, cols = len(board), len(board[0])

    def dfs(r, c, i):
        if i == len(word):  # All letters matched
            return True
        if (
            r < 0 or c < 0 or r >= rows or c >= cols or
            board[r][c] != word[i]
        ):
            return False

        temp = board[r][c]
        board[r][c] = '#'  # Mark as visited

        # Explore all 4 directions
        found = (
            dfs(r+1, c, i+1) or
            dfs(r-1, c, i+1) or
            dfs(r, c+1, i+1) or
            dfs(r, c-1, i+1)
        )

        board[r][c] = temp  # Restore for backtracking
        return found

    for r in range(rows):
        for c in range(cols):
            if board[r][c] == word[0] and dfs(r, c, 0):
                return True

    return False
```

---

### 🧪 Dry Run on Example 1

```txt
Word: ABCCED
Start from board[0][0] = 'A' → match
Go right → 'B' → match
Go right → 'C' → match
Go down → 'C' → match
Go down → 'E' → match
Go left → 'D' → match → Word found ✅
```

---

### 🧰 Optimization (Search Pruning)

We can apply **search pruning** by checking whether the board contains enough of each character to even form the word.

#### 🔎 Pre-check Optimization:

```python
from collections import Counter

if len(word) > rows * cols:
    return False

board_letters = Counter(char for row in board for char in row)
word_letters = Counter(word)

for ch in word_letters:
    if word_letters[ch] > board_letters.get(ch, 0):
        return False
```

Also, we can **reverse the word** if it starts with a less frequent character, increasing the chance of early failure.

---

Let me know if you'd like this saved into a `.md` file or want the solution in a different language!