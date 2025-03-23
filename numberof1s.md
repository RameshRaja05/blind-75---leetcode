### **Counting Set Bits (Hamming Weight)**
We need to count the number of **1s** in the **binary representation** of a given integer `n`. Here are different approaches:

---

## **1️⃣ Brian Kernighan’s Algorithm (Most Efficient)**
This algorithm removes the **rightmost set bit** (`1`) in each step using:
\[
n = n \& (n-1)
\]
Since each step removes one `1`, the loop runs only as many times as there are `1s` in `n`.

### **Code (Python)**
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n:
            n &= (n - 1)  # Removes the lowest set bit
            count += 1
        return count
```
✅ **Time Complexity**: \( O(k) \) (where `k` is the number of set bits, at most 32)  
✅ **Space Complexity**: \( O(1) \)  

---

## **2️⃣ Bitwise Shift Approach**
- Check the **last bit** using `n & 1`  
- Right shift `n` (`n >>= 1`) until `n` becomes `0`.

### **Code (Python)**
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n:
            count += (n & 1)  # Check last bit
            n >>= 1  # Shift right
        return count
```
✅ **Time Complexity**: \( O(\log n) \) (~32 iterations for a 32-bit integer)  
✅ **Space Complexity**: \( O(1) \)  

---

## **3️⃣ Python’s Built-in Function**
Python provides an easy way to count `1s` using `bin(n).count('1')`.

### **Code (Python)**
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        return bin(n).count('1')
```
✅ **Time Complexity**: \( O(1) \)  
✅ **Space Complexity**: \( O(1) \)  

---

### **Optimization for Multiple Calls**
If this function is called many times, **precompute the Hamming weight** for small values (like a byte) and use **bitwise masking**:
1. **Use a lookup table** (precompute results for `0-255`).
2. **Process 8 bits (1 byte) at a time** for efficiency.

Would you like an implementation for this? 🚀