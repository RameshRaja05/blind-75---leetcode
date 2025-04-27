---

# 🧮 Maximum Product Subarray

## 📝 Problem

Given an integer array `nums`, find the contiguous subarray **within an array** (containing at least one number) which has the **largest product**, and return the product.

> Constraints:
- 1 <= nums.length <= 2 * 10⁴
- -10 <= nums[i] <= 10
- The product of any subarray fits in a 32-bit integer.

---

## 💡 Intuition

Multiplying two negatives becomes positive — so we need to track both:
- The **maximum product so far**
- The **minimum product so far** (could become the max later if another negative number is multiplied)

---

## 🔍 Approach: Dynamic Programming (Kadane's Variant)

### ✅ Step-by-Step Strategy

1. Initialize:
   - `max = max(nums)` (handles edge cases like single negative number)
   - `currentMax = 1`, `currentMin = 1`

2. Traverse each number in array:
   - Store temporary product: `temp = currentMax * num`
   - Update:
     - `currentMax = max(temp, num, currentMin * num)`
     - `currentMin = min(temp, num, currentMin * num)`
   - Update result: `max = max(max, currentMax)`

---

## 💻 Java Implementation (Alternative Version)

```java
class Solution {
    public int maxProduct(int[] nums) {
        int max = Arrays.stream(nums).max().getAsInt();

        int currentMax = 1;
        int currentMin = 1;

        for(int num : nums) {
            int temp = currentMax * num;
            currentMax = Math.max(temp, Math.max(currentMin * num, num));
            currentMin = Math.min(temp, Math.min(currentMin * num, num));

            max = Math.max(max, currentMax);
        }

        return max;
    }
}
```

---

## 📈 Visual Example

Input: `nums = [2, 3, -2, 4]`

```text
Index      nums[i]     currentMax     currentMin     max
---------------------------------------------------------
0             2            2              2           2
1             3            6              3           6 ✅
2            -2           -2            -12           6
3             4            4            -48           6 ✅
```

Answer: `6`

---

## 📊 Complexity

| Metric | Value        |
|--------|--------------|
| Time   | O(n)         |
| Space  | O(1)         |

---

## 🧪 Additional Examples

### Example 1
```text
Input: nums = [2, 3, -2, 4]
Output: 6
```

### Example 2
```text
Input: nums = [-2, 0, -1]
Output: 0
Note: Cannot use [-2, -1] as it's not contiguous due to 0.
```

---

Would you like a diagram or trace table that shows the max/min values evolving over time?

It seems the update didn't reflect properly. Let me provide a clearer version of the approach you requested:

---

### 🧮 Approach 2: Forward and Backward Pass (Handling Negative Numbers)

### ✅ Step-by-Step Strategy

1. **Forward pass**: Traverse from left to right.
   - Track a running product `prod` and update `maxSoFar`.
   - Reset `prod` to 1 if it reaches 0 (to avoid continuing a broken product).

2. **Backward pass**: Traverse from right to left.
   - Similarly track a running product `prod` from the right side.
   - Reset `prod` to 1 if it reaches 0.

3. The result will be the maximum of the two passes.

### 💻 Java Implementation (Second Approach)

```java
class Solution {
    public int maxProduct(int[] nums) {
        int prod = 1;
        int maxSoFar = Integer.MIN_VALUE;

        // Forward pass: left to right
        for(int i = 0; i < nums.length; i++) {
            prod *= nums[i];
            maxSoFar = Math.max(prod, maxSoFar);
            if(prod == 0) {
                prod = 1; // Reset product if it becomes zero
            }
        }

        prod = 1;

        // Backward pass: right to left
        for(int i = nums.length - 1; i >= 0; i--) {
            prod *= nums[i];
            maxSoFar = Math.max(prod, maxSoFar);
            if(prod == 0) {
                prod = 1; // Reset product if it becomes zero
            }
        }

        return maxSoFar;
    }
}
```

---

Would you like any further explanations or additional visualizations for this approach?