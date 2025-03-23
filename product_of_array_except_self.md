**Problem Statement:**
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

**Constraints:**
- `2 <= nums.length <= 10⁵`
- `-30 <= nums[i] <= 30`
- The input is generated such that `answer[i]` is guaranteed to fit in a 32-bit integer.

**Follow-up:**
- Can you solve the problem in **O(1)** extra space complexity? (The output array does not count as extra space for space complexity analysis.)

---

**Example 1:**

**Input:** nums = `[1,2,3,4]`  
**Output:** `[24,12,8,6]`  

**Example 2:**

**Input:** nums = `[-1,1,0,-3,3]`  
**Output:** `[0,0,9,0,0]`  

---

**Approach:**

Since we are not allowed to use division, we can solve this problem using **prefix and suffix products** in O(n) time.

### **Two-Pass Solution Using Prefix and Suffix Arrays**

1. Compute the `prefix` product for each element:
   - `prefix[i]` stores the product of all elements before `nums[i]`.
2. Compute the `suffix` product for each element:
   - `suffix[i]` stores the product of all elements after `nums[i]`.
3. The result at each index is `prefix[i] * suffix[i]`.

**Time Complexity:** O(N)  
**Space Complexity:** O(N) (can be optimized to O(1))

**Implementation (Python):**

```python
def productExceptSelf(nums):
    n = len(nums)
    answer = [1] * n
    
    prefix = 1
    for i in range(n):
        answer[i] = prefix
        prefix *= nums[i]
    
    suffix = 1
    for i in range(n - 1, -1, -1):
        answer[i] *= suffix
        suffix *= nums[i]
    
    return answer
```

### **Optimized Space Solution (O(1) Extra Space)**

Instead of using separate `prefix` and `suffix` arrays, we store the prefix product directly in the result array and compute the suffix product in a second pass.

1. Initialize `answer` where `answer[i]` stores the prefix product.
2. Traverse `nums` from left to right to populate prefix products.
3. Traverse `nums` from right to left to multiply suffix products.

**Time Complexity:** O(N)  
**Space Complexity:** O(1) (excluding output array)

**Implementation (Python):**

```python
from typing import List

def productExceptSelf(nums: List[int]) -> List[int]:
    n = len(nums)
    answer = [1] * n
    
    prefix = 1
    for i in range(n):
        answer[i] = prefix
        prefix *= nums[i]
    
    suffix = 1
    for i in range(n - 1, -1, -1):
        answer[i] *= suffix
        suffix *= nums[i]
    
    return answer
```

This approach efficiently computes the result in **O(n) time** while using **O(1) extra space** (excluding the output array).

