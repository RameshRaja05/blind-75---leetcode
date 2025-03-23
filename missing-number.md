**Problem Statement:**
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

**Constraints:**
- `n == nums.length`
- `1 <= n <= 10^4`
- `0 <= nums[i] <= n`
- All the numbers in `nums` are unique.

**Example 1:**
```
Input: nums = [3,0,1]
Output: 2
Explanation: 
n = 3 since there are 3 numbers, so all numbers are in the range [0,3].
2 is the missing number in the range since it does not appear in nums.
```

**Example 2:**
```
Input: nums = [0,1]
Output: 2
Explanation: 
n = 2 since there are 2 numbers, so all numbers are in the range [0,2].
2 is the missing number in the range since it does not appear in nums.
```

**Example 3:**
```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: 
n = 9 since there are 9 numbers, so all numbers are in the range [0,9].
8 is the missing number in the range since it does not appear in nums.
```

---

**Approach:**
To find the missing number efficiently, we use the mathematical **sum formula** for the first `n` natural numbers.

**Algorithm:**
1. Compute the expected sum of numbers from `0` to `n` using the formula:
   - `totalSum = (n * (n + 1)) / 2`
2. Compute the actual sum of the given `nums` array.
3. The missing number is `totalSum - sum`.

This approach ensures an optimal solution with **O(n) time complexity** and **O(1) space complexity**.

---

**Java Solution:**
```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int totalSum = (n * (n + 1)) / 2;
        int sum = 0;

        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }

        return totalSum - sum;
    }
}
```

---

**Time Complexity:**
- **O(n)**: The loop iterates over the array once to compute the sum.

**Space Complexity:**
- **O(1)**: Only a few integer variables are used, requiring constant extra space.

This method provides an efficient way to determine the missing number without additional space overhead.