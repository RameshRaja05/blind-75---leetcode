**# House Robber Problem**

## **Problem Statement**
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. However, adjacent houses have security systems connected, meaning if two adjacent houses are robbed on the same night, the police will be alerted.

Given an integer array `nums` representing the amount of money at each house, return the **maximum amount** of money you can rob without alerting the police.

### **Example 1:**
**Input:** `nums = [1,2,3,1]`  
**Output:** `4`  
**Explanation:** Rob house 1 (money = 1) and house 3 (money = 3).  
Total amount = `1 + 3 = 4`.

### **Example 2:**
**Input:** `nums = [2,7,9,3,1]`  
**Output:** `12`  
**Explanation:** Rob house 1 (money = 2), house 3 (money = 9), and house 5 (money = 1).  
Total amount = `2 + 9 + 1 = 12`.

### **Constraints:**
- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

---

## **Approach: Dynamic Programming (Bottom-Up)**

### **Observations:**
1. If we rob house `i`, we **cannot** rob house `i-1`.
2. At house `i`, we have two choices:
   - **Rob it**: Take `nums[i]` and add it to `dp[i-2]`.
   - **Skip it**: Take `dp[i-1]`.
3. We define `dp[i]` as the **maximum money** that can be robbed up to house `i`.

### **Recurrence Relation:**
\[ dp[i] = \max(dp[i-1], nums[i] + dp[i-2]) \]

### **Base Cases:**
- `dp[0] = nums[0]` (Only one house, rob it)
- `dp[1] = max(nums[0], nums[1])` (Choose the max of the first two houses)

### **Implementation:**
#### **1️⃣ Dynamic Programming with an Array**
We use an array `dp[]` to store intermediate results.

```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
        }
        
        return dp[nums.length - 1];
    }
}
```

✅ **Time Complexity:** \( O(n) \)  
✅ **Space Complexity:** \( O(n) \)

---

#### **2️⃣ Optimized Dynamic Programming (Space Efficient)**
Instead of using an array, we only keep track of **two previous values**, reducing space complexity to \( O(1) \).

```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        
        int prev2 = 0, prev1 = 0;
        
        for (int num : nums) {
            int newRob = Math.max(prev1, num + prev2);
            prev2 = prev1;
            prev1 = newRob;
        }
        
        return prev1;
    }
}
```

✅ **Time Complexity:** \( O(n) \)  
✅ **Space Complexity:** \( O(1) \)  

---

## **Which Approach Should You Use?**
| Approach | Time Complexity | Space Complexity | When to Use? |
|----------|---------------|----------------|--------------|
| **Dynamic Programming (Array)** | \( O(n) \) | \( O(n) \) | Good for understanding DP |
| **Optimized DP (Two Variables)** | \( O(n) \) | \( O(1) \) | Best for large `n` (recommended) |

Both approaches have the same time complexity, but the **optimized DP approach** is preferred in real-world scenarios because it **reduces memory usage**.

Would you like a **recursive** solution as well? 🚀

