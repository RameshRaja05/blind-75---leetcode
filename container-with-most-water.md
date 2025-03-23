**Problem Statement:**
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i-th` line are `(i, 0)` and `(i, height[i])`.

Find two lines that, together with the x-axis, form a container such that the container holds the most water.

Return the maximum amount of water a container can store.

**Constraints:**
- `n == height.length`
- `2 <= n <= 10^5`
- `0 <= height[i] <= 10^4`

---

**Example 1:**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The max area of water the container can contain is 49.
```

**Example 2:**
```
Input: height = [1,1]
Output: 1
```

---

**Approach:**
To solve this problem efficiently, we use the **two-pointer approach**.

**Algorithm:**
1. Initialize two pointers, `l` (left) at the start and `r` (right) at the end of the array.
2. Calculate the area between `height[l]` and `height[r]` using the formula: `area = (r - l) * min(height[l], height[r])`.
3. Update `max_area` if the calculated area is greater than the current `max_area`.
4. Move the pointer pointing to the shorter line inward since increasing the width alone will not increase the area if the height remains the same.
5. Repeat until `l` meets `r`.
6. Return `max_area`.

---

**Python Solution:**
```python
from typing import List

class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height) - 1
        max_area = 0
        
        while l < r:
            max_area = max(max_area, (r - l) * min(height[l], height[r]))
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        
        return max_area
```

---

**Time Complexity:**
- **O(n)**: We traverse the array only once using the two-pointer technique.

**Space Complexity:**
- **O(1)**: No additional space is used apart from a few integer variables.

This approach ensures an optimal and efficient solution for finding the maximum water that can be stored in a container.

