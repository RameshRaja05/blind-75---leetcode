### **Climbing Stairs - Dynamic Programming Approach**  
This problem follows the **Fibonacci pattern**.  
- If you are at step `n`, you must have arrived from either:
  - Step `n-1` (taking **1 step**)
  - Step `n-2` (taking **2 steps**)  
- Therefore, the recurrence relation is:  
  \[
  f(n) = f(n-1) + f(n-2)
  \]
  With base cases:  
  - \( f(1) = 1 \) (1 way to climb 1 step)
  - \( f(2) = 2 \) (2 ways to climb 2 steps)

---

### **1️⃣ Dynamic Programming (Bottom-Up)**
We use an **array** to store intermediate results.

#### **Code (Python)**
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        dp = [0] * (n + 1)
        dp[1], dp[2] = 1, 2
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n]
```
✅ **Time Complexity**: \( O(n) \)  
✅ **Space Complexity**: \( O(n) \)  

---

### **2️⃣ Optimized DP (Space-Efficient)**
Instead of using an array, we only keep the last two values.

#### **Code (Python)**
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        first, second = 1, 2
        for _ in range(3, n + 1):
            first, second = second, first + second
        return second
```
✅ **Time Complexity**: \( O(n) \)  
✅ **Space Complexity**: \( O(1) \)  

---

### **3️⃣ Mathematical Approach (Formula)**
Since the problem follows the **Fibonacci sequence**, we can use **Binet's Formula**:
\[
f(n) = \frac{\phi^n - \psi^n}{\sqrt{5}}
\]
where:
- \( \phi = \frac{1 + \sqrt{5}}{2} \) (Golden ratio)
- \( \psi = \frac{1 - \sqrt{5}}{2} \)  

#### **Code (Python)**
```python
import math
class Solution:
    def climbStairs(self, n: int) -> int:
        sqrt5 = math.sqrt(5)
        phi = (1 + sqrt5) / 2
        psi = (1 - sqrt5) / 2
        return int((phi**(n + 1) - psi**(n + 1)) / sqrt5)
```
✅ **Time Complexity**: \( O(1) \)  
✅ **Space Complexity**: \( O(1) \)  

---

### **Which One Should You Use?**
| Approach | Time Complexity | Space Complexity | When to Use? |
|----------|---------------|----------------|--------------|
| **Dynamic Programming (Array)** | \( O(n) \) | \( O(n) \) | Good for small values of `n` |
| **Optimized DP (Two Variables)** | \( O(n) \) | \( O(1) \) | Best for large `n` (recommended) |
| **Mathematical Formula** | \( O(1) \) | \( O(1) \) | Fastest, but may lose precision for large `n` |

Would you like help implementing a recursive approach? 🚀