## Add Two Integers Without Using `+` or `-`

### Problem Statement
Given two integers `a` and `b`, return the sum of the two integers **without** using the arithmetic operators `+` and `-`.

---

### Examples

**Example 1:**
```
Input: a = 1, b = 2  
Output: 3
```

**Example 2:**
```
Input: a = 2, b = 3  
Output: 5
```

---

### Constraints
- `-1000 <= a, b <= 1000`

---

### Approach
To perform addition without using the `+` or `-` operators, we simulate the process of binary addition using bitwise operations. The key to this approach lies in understanding how binary addition works:

1. **XOR (`^`)** simulates addition without considering the carry.
2. **AND (`&`) followed by a left shift (`<<`)** identifies where the carry occurs and shifts it left so it can be added in the next iteration.

Let’s walk through the approach in detail:

#### Step-by-Step Logic

1. **Sum Without Carry (`a ^ b`)**:  
   The XOR operation returns the bit-level sum of `a` and `b`, ignoring any carry. For instance:
   - `1 ^ 0 = 1`
   - `1 ^ 1 = 0` (because there's a carry, which we will handle separately)

2. **Calculate the Carry (`(a & b) << 1`)**:  
   The AND operation returns the bits where both `a` and `b` have 1s — i.e., where carry will be generated. Left-shifting this result by 1 aligns it to the next higher bit position, where the carry will be added in the next iteration.

3. **Update `a` and `b`**:  
   Replace `a` with the new partial sum (from XOR) and `b` with the new carry. Repeat the process until the carry becomes zero.

4. **Return Result**:  
   Once the carry is zero, `a` holds the final result. In Python, because integers are of arbitrary size, we use bitmasking to simulate 32-bit overflow and handle negative numbers using two’s complement logic.

This bitwise technique effectively reproduces how hardware circuits perform binary addition, such as in an Arithmetic Logic Unit (ALU).

---

### Python Implementation
```python
def getSum(a: int, b: int) -> int:
    mask = 0xFFFFFFFF
    max_int = 0x7FFFFFFF

    while b != 0:
        sum_ = (a ^ b) & mask
        carry = ((a & b) << 1) & mask
        a, b = sum_, carry

    return a if a <= max_int else ~(a ^ mask)
```

---

### Java Implementation
```java
public class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int sum = a ^ b;
            int carry = (a & b) << 1;
            a = sum;
            b = carry;
        }
        return a;
    }
}
```

---

### Notes
- In Java, integer overflow is automatically handled with 32-bit 2's complement, so no need for manual masking like in Python.
- This method works for both positive and negative integers within the given constraint.

