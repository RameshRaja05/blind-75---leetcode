# LeetCode 33. Search in Rotated Sorted Array

## 🧩 Problem Description
You are given an integer array `nums` sorted in ascending order (with **distinct values**), and an integer `target`.

The array was **rotated** at an unknown pivot index. Find the index of `target` if it is in the array, or return `-1` if it is not.

### Example:
```text
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

### Constraints:
- All values are distinct.
- Must run in **O(log n)** time.

---

## 🔍 Step-by-Step Walkthrough

### ✅ Key Observations
- Rotated array has two **sorted halves**.
- One half will always be sorted.
- Binary search can still be used by determining which half is sorted.

### 🧠 Binary Search Strategy
1. Use a while loop to perform binary search.
2. At each step, determine which half is sorted:
   - If `nums[left] <= nums[mid]`, then left side is sorted.
   - Else, right side is sorted.
3. Narrow the search to the half that **may** contain the target.

---

## 🔧 Visual Example

For `nums = [4,5,6,7,0,1,2]`, `target = 0`
```
left = 0, right = 6
mid = 3 → nums[mid]=7
nums[left]=4 <= nums[mid]=7 → left is sorted
→ target not in left → move left to mid+1

left = 4, right = 6
mid = 5 → nums[mid]=1
nums[left]=0 <= nums[mid]=1 → left is sorted
→ target in left → move right to mid

left = 4, right = 5
mid = 4 → nums[mid]=0 → found target at index 4
```

---

## 💡 Algorithm Details

### Python Implementation
```python
from typing import List

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            
            if nums[left] <= nums[mid]:  # left half is sorted
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:  # right half is sorted
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        
        return -1
```

### Java Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;

            if (nums[left] <= nums[mid]) { // left side sorted
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else { // right side sorted
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

---

## 🧠 Algorithm Explanation
1. **Initialize** two pointers: `left` at 0 and `right` at `n - 1`.
2. **Binary Search Loop** until `left <= right`:
   - Find the `mid` index.
   - If `nums[mid] == target`, return `mid`.
   - Identify the **sorted half**:
     - If `nums[left] <= nums[mid]`, the left half is sorted.
     - Else, the right half is sorted.
   - Determine if the target lies within the sorted half:
     - If yes, narrow the search to that half.
     - If not, search the other half.
3. **Return -1** if the loop ends without finding the target.

---

## 🧪 Edge Cases
- No rotation (purely sorted): `[1,2,3,4,5]`, target = 3 → return index
- Full rotation: `[1]`, target = 1 → return 0
- Target not present: `[4,5,6,7,0,1,2]`, target = 3 → return -1

---

## ⏱ Time and Space Complexity
- **Time:** O(log n) → Binary search halves the array
- **Space:** O(1) → No extra space used

---

## ✅ Final Notes
- Handles rotated arrays while preserving O(log n) time.
- Focus is on identifying the **sorted region** in each step.
- Works great as a base pattern for many rotated array variations.

