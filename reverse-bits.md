**Problem Statement:**
Reverse the bits of a given 32-bit unsigned integer.

---

**Notes:**
- In some languages, such as Java, there is no unsigned integer type. The input and output will still be treated as signed integers, but the internal binary representation remains the same.
- Java represents signed integers using **2’s complement notation**. Thus, negative numbers will still be correctly reversed when considering their bitwise representation.

---

**Example 1:**

**Input:** n = `00000010100101000001111010011100`  
**Output:** `964176192` (`00111001011110000010100101000000`)  
**Explanation:** The input binary string `00000010100101000001111010011100` represents the unsigned integer `43261596`, so return `964176192`, which has a reversed binary representation of `00111001011110000010100101000000`.

**Example 2:**

**Input:** n = `11111111111111111111111111111101`  
**Output:** `3221225471` (`10111111111111111111111111111111`)  
**Explanation:** The input binary string `11111111111111111111111111111101` represents the unsigned integer `4294967293`, so return `3221225471`, which has a reversed binary representation of `10111111111111111111111111111111`.

---

**Constraints:**
- The input must be a **binary string of length 32**.

---

**Approach:**

### **Bit Manipulation (O(1) Time Complexity)**
Since we are dealing with a fixed 32-bit integer, we can use bitwise operations to reverse the bits efficiently.

### **Steps:**
1. Initialize `result = 0`.
2. Iterate through all 32 bits of `n`.
3. Extract the `i`-th bit from `n` using `(n >> i) & 1`.
4. Place this extracted bit in its reversed position using `bit << (31 - i)`, then OR it with `result`.
5. Repeat this process for all 32 bits.

**Time Complexity:** O(1) (Fixed 32-bit loop)  
**Space Complexity:** O(1)  

### **Implementation (Python):**

```python
def reverseBits(n: int) -> int:
    result = 0
    for i in range(32):
        bit = (n >> i) & 1  # Extract the i-th bit
        result |= (bit << (31 - i))  # Place it in the reversed position
    return result
```

### **Implementation (Java):**

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int reverseBits(int n) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int bit = (n >> i) & 1; // Extract the i-th bit
            ans = ans | (bit << (31 - i)); // Place it in the reversed position
        }
        return ans;
    }
}
```

---

### **Follow-Up Optimization:**
If this function is called many times, we can use **memoization** to store already computed results for common inputs, reducing computation time for repeated calls.

This approach ensures an efficient and optimized solution for reversing bits in a 32-bit integer. 🚀

