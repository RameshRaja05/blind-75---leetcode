
# 🏃 LeetCode 55: Jump Game

## 📝 Problem Statement

You are given an integer array `nums`. Each element `nums[i]` represents the maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

---

## ✅ Example

```text
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

```text
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to move further.
```

---

## 💡 Approach

This is a **Greedy Algorithm** problem.

We can solve this in two ways:

---

### 🔍 Forward Greedy (Max Reach Approach)

1. Initialize `maxReach = 0`.
2. Traverse the array:
   - If `i > maxReach`, return `false` (stuck).
   - Update `maxReach = max(maxReach, i + nums[i])`.
3. If loop completes, return `true`.

✅ Always try to jump as far as possible.

---

### 🔄 Reverse Greedy (Your Version)

1. Start from the end (`goal = nums.length - 1`).
2. Iterate backward:
   - If `i + nums[i] >= goal`, set `goal = i`.
3. At the end, return `goal == 0`.

✅ If we can reduce the goal to 0, we can reach the end from the start.

---

## 🧪 Test Case Walkthrough

**Input:** `nums = [2, 3, 1, 1, 4]`

Forward Greedy:
- i=0: maxReach = max(0,0+2)=2
- i=1: maxReach = max(2,1+3)=4
- i=2: maxReach = max(4,2+1)=4
- i=3: maxReach = max(4,3+1)=4
- i=4: maxReach = max(4,4+4)=8 → ✅ Reached

Reverse Greedy:
- goal = 4
- i=3: 3+1 ≥ 4 → goal = 3
- i=2: 2+1 ≥ 3 → goal = 2
- i=1: 1+3 ≥ 2 → goal = 1
- i=0: 0+2 ≥ 1 → goal = 0 → ✅ Reached

---

## ⏱️ Complexity

| Type             | Complexity |
|------------------|------------|
| Time Complexity  | O(n)       |
| Space Complexity | O(1)       |

---

## 🧾 Code

### ✅ Python

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        maxReach = 0
        for i, num in enumerate(nums):
            if i > maxReach:
                return False
            maxReach = max(maxReach, i + num)
        return True
```

---

### ✅ Java — Forward Greedy

```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) return false;
            maxReach = Math.max(maxReach, i + nums[i]);
        }
        return true;
    }
}
```

---

### ✅ Java — Reverse Greedy (Your Version)

```java
class Solution {
    public boolean canJump(int[] nums) {
        int goal = nums.length - 1;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (i + nums[i] >= goal) {
                goal = i;
            }
        }
        return goal == 0;
    }
}
```

---

## 📌 Key Takeaway

- Greedy algorithms work by making local optimal choices.
- Both forward and reverse greedy solve this with O(n) time and constant space.
