# LeetCode 153. Find Minimum in Rotated Sorted Array

## 🧩 Problem Description
Suppose an array of length `n` sorted in ascending order is **rotated** between 1 and `n` times. For example:
```text
[0,1,2,4,5,6,7] → rotated 4 times → [4,5,6,7,0,1,2]
```

Find the minimum element.

### Assumptions:
- No duplicate elements exist.
- You must solve it in **O(log n)** time.

---

## 🔍 Step-by-Step Walkthrough

### ✅ Observation
- The rotated sorted array has two sorted halves.
- The minimum element is the **pivot point** (the only place where `arr[i] > arr[i+1]`).

### 🧠 Binary Search Idea
- Use binary search to shrink the window:
  - Compare mid with the rightmost element:
    - If `mid > right`: min must be on the right side.
    - Else: min is on the left or could be `mid`.

---

## 🔧 Visual Example

For input:
```text
[4,5,6,7,0,1,2]
```
Binary search trace:
```
left = 0, right = 6
mid = 3 → nums[mid]=7, nums[right]=2 → move left to mid+1
left = 4, right = 6
mid = 5 → nums[mid]=1, nums[right]=2 → move right to mid
left = 4, right = 5
mid = 4 → nums[mid]=0, nums[right]=1 → move right to mid
left = 4, right = 4 → return nums[left]=0
```

---

## 💡 Algorithm Details

### Python Implementation
```python
from typing import List

class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1
        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
        return nums[left]
```

### Java Implementation (Classic Style)
```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return nums[left];
    }
}
```

---

## 🔄 Alternative Java Implementation (Explicit Sorted Check)
This version checks if the current portion is already sorted and avoids further binary search:

```java
class Solution {
    public int findMin(int[] nums) {
        int min = nums[0];
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            // Case 1: already sorted
            if (nums[left] < nums[right]) {
                min = Math.min(min, nums[left]);
                break;
            }

            int mid = (left + right) >>> 1;
            min = Math.min(min, nums[mid]);

            // Case 2: mid is in left part
            if (nums[mid] >= nums[left]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return min;
    }
}
```

### 🧠 Algorithm Explanation (Explicit Sorted Check Approach)
1. **Initialization:** Start with `min = nums[0]` and standard binary search boundaries `left` and `right`.
2. **Sorted Check:** In each iteration, if the current subarray (`nums[left]` to `nums[right]`) is sorted (i.e., `nums[left] < nums[right]`), then the smallest value is `nums[left]`. Update `min` and exit the loop.
3. **Update min:** Always update `min` with `nums[mid]` to track the lowest value found.
4. **Left Half vs Right Half:**
   - If `nums[mid] >= nums[left]`, it means the left half is sorted, so the pivot (and hence the minimum) must be in the right half → shift `left = mid + 1`.
   - Else, the right half is unsorted and may contain the pivot → shift `right = mid - 1`.
5. **Return min:** The loop ends when `left > right`, and the smallest value is stored in `min`.

---

## 🔁 Comparison of Strategies
| Strategy | How it works | Condition checked |
|---------|---------------|-------------------|
| Classic | Compare `mid` to `right` | `nums[mid] > nums[right]` |
| Explicit Sorted | Check if current range is sorted | `nums[left] < nums[right]` |

---

## 🧪 Edge Cases
- Array not rotated: `[1,2,3,4,5]` → min is `1`
- Array of 1 element: `[2]` → min is `2`
- Rotated at the middle: `[3,4,5,1,2]` → min is `1`

---

## ⏱ Time and Space Complexity
- **Time:** O(log n) → Binary search
- **Space:** O(1) → No extra space used

---

## ✅ Final Notes
- Both strategies are valid and efficient.
- Choose the one that feels intuitive in interviews.
- Works great for rotated sorted arrays — a pattern seen in many follow-up problems.

