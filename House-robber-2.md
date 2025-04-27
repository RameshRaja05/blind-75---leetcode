
---

## 🏠 House Robber II (Circular Houses)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. Houses are arranged **in a circle**, so the first and last are adjacent.

You can't rob two adjacent houses, or an alarm will go off.

---

### 🔍 Key Insight

Because the first and last houses are adjacent, **we can't rob both**. So we split into two linear scenarios:

- **Case 1:** Rob from index `0` to `n - 2`
- **Case 2:** Rob from index `1` to `n - 1`

Return the **maximum** of the two results.

---

### ✅ Java Code

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0]; // Only one house

        return Math.max(robLinear(nums, 0, n - 2), robLinear(nums, 1, n - 1));
    }

    private int robLinear(int[] nums, int start, int end) {
        int prev1 = 0, prev2 = 0;

        for (int i = start; i <= end; i++) {
            int temp = Math.max(prev1, prev2 + nums[i]);
            prev2 = prev1;
            prev1 = temp;
        }

        return prev1;
    }
}
```

---

### 🧪 Example 1

Input: `nums = [2,3,2]`

```
Case 1 (0 to 1): [2,3] => Max = 3
Case 2 (1 to 2): [3,2] => Max = 3

✅ Result: 3
```

---

### 🧪 Example 2

Input: `nums = [1,2,3,1]`

```
Case 1 (0 to 2): [1,2,3] => Max = 4 (1 + 3)
Case 2 (1 to 3): [2,3,1] => Max = 3

✅ Result: 4
```

---

### 🧠 Time and Space Complexity

- **Time Complexity:** `O(n)` — each house is visited once per case
- **Space Complexity:** `O(1)` — uses only a few variables

---

### 📌 Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`
