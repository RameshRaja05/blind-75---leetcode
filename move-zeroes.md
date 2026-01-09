# LeetCode 283: Move Zeroes

## Problem Statement

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

Do this **in-place**, without making a copy of the array.

---

## Example:

**Input:**

```java
nums = [0,1,0,3,12]
```

**Output:**

```
[1,3,12,0,0]
```

---

## Constraints

* `1 <= nums.length <= 10^4`
* `-2^31 <= nums[i] <= 2^31 - 1`

---

## Approach 1: Two Pointers with Overwrite

Maintain a `nonZeroIndex` pointer and overwrite from the left as we find non-zero elements. After the loop, set remaining values to 0.

### Code:

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int nonZeroIndex = 0;

        // Move all non-zero values to the front
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[nonZeroIndex] = nums[i];
                nonZeroIndex++;
            }
        }

        // Fill remaining positions with zeroes
        for (int i = nonZeroIndex; i < nums.length; i++) {
            nums[i] = 0;
        }
    }
}
```

---

## Approach 2: Two Pointers with In-place Swap

This version **swaps** the zero with the next non-zero element using two pointers.

### Key Idea

* `l` points to where the next non-zero element should go.
* `r` moves forward to find non-zero elements.
* If `nums[r] != 0`, swap `nums[l]` and `nums[r]`, then increment `l`.

### Code:

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int l = 0;
        for (int r = 0; r < nums.length; r++) {
            if (nums[r] != 0) {
                int temp = nums[r];
                nums[r] = nums[l];
                nums[l] = temp;
                l++;
            }
        }
    }
}
```

---

## Dry Run (for Swap Approach)

Input: `nums = [0,1,0,3,12]`

1. `r = 0`, `nums[0] = 0` → skip
2. `r = 1`, `nums[1] = 1` → swap with `nums[0]` → `[1,0,0,3,12]`, `l = 1`
3. `r = 2`, `nums[2] = 0` → skip
4. `r = 3`, `nums[3] = 3` → swap with `nums[1]` → `[1,3,0,0,12]`, `l = 2`
5. `r = 4`, `nums[4] = 12` → swap with `nums[2]` → `[1,3,12,0,0]`, `l = 3`

---

## Time & Space Complexity (Both Approaches)

* **Time Complexity:** `O(N)` – single traversal of the array.
* **Space Complexity:** `O(1)` – constant space, in-place operations.

---

## Comparison and Final Thoughts

| Criteria             | Overwrite Approach          | Swap Approach                |
| -------------------- | --------------------------- | ---------------------------- |
| **Simplicity**       | ✅ Easier to read            | 🔄 Slightly more complex     |
| **Write Efficiency** | ✅ Fewer writes              | ❌ More writes due to swaps   |
| **Pointer Training** | 🔸 Less focus               | ✅ Great for pointer practice |
| **Best Use Case**    | Production, SSD-safe writes | Simulating real movements    |

> 🔍 **Recommendation**: Use **overwrite** for fewer writes and performance. Use **swap** when learning pointer logic or when swaps better represent the problem intent.
