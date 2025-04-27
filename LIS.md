# 🧮 Longest Increasing Subsequence - Java Solutions

## 📝 Problem Statement

> Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.

### 💡 Example

```text
Input:  nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: LIS = [2,3,7,101]
```

---

## 🚀 Approach 1: Dynamic Programming (O(n²))

### 📌 Code

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] LIS = new int[n];
        Arrays.fill(LIS, 1);

        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                if (nums[j] > nums[i]) {
                    LIS[i] = Math.max(LIS[i], 1 + LIS[j]);
                }
            }
        }

        return Arrays.stream(LIS).max().getAsInt();
    }
}
```

### 🔍 Step-by-Step Walkthrough

1. **Initialization**: Create an array `LIS[]` of size `n` (same as input size), and fill it with 1s because every element is a subsequence of length 1.
2. **Reverse Iteration**: Loop backwards from `i = n - 1` to `0`. This ensures we’re calculating future LIS values before current ones.
3. **Forward Comparison**: For every index `i`, compare with every `j > i`:
   - If `nums[j] > nums[i]`, then `nums[j]` can extend the subsequence starting at `i`.
   - Update `LIS[i] = max(LIS[i], 1 + LIS[j])`.
4. **Result**: The final answer is the maximum value in `LIS[]`.

### 📉 Time & Space

- Time: `O(n²)`
- Space: `O(n)`

### 🧠 Visual

```text
nums: [10, 9, 2, 5, 3, 7, 101, 18]
LIS : [4, 3, 4, 3, 3, 2, 1, 1]
            ↑       ↑       ↑
        increasing subsequence counts
```

---

## ⚡ Approach 2: Binary Search + Greedy (O(n log n))

### 📌 Code

```java
class Solution {
    public int binarySearch(List<Integer> res, int n) {
        int left = 0;
        int right = res.size() - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            int curElement = res.get(mid);
            if (curElement == n) {
                return mid;
            } else if (curElement < n) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return left;
    }

    public int lengthOfLIS(int[] nums) {
        List<Integer> res = new ArrayList<>();

        for (int num : nums) {
            if (res.isEmpty() || res.get(res.size() - 1) < num) {
                res.add(num);
            } else {
                int i = binarySearch(res, num);
                res.set(i, num);
            }
        }

        return res.size();
    }
}
```

### 🔍 Step-by-Step Walkthrough

1. **Initialization**: Create an empty list `res`. This will track potential end values of increasing subsequences.
2. **Iterate over `nums`**:
   - If `res` is empty or `num > res.last`, append `num` to `res`.
   - Else, perform binary search in `res` to find the index where `num` can replace a larger or equal element. This keeps the list optimal.
3. **Binary Search Details**:
   - We return the **insertion index** (first element `≥ num`) using custom binary search.
   - This ensures `res` always holds smallest possible tail elements.
4. **Result**: The length of `res` is the length of the LIS.

### 📉 Time & Space

- Time: `O(n log n)`  
- Space: `O(n)`

### 📈 Visual

```text
nums: [10, 9, 2, 5, 3, 7, 101, 18]

Step-by-step 'res':
[10]
[9]
[2]
[2, 5]
[2, 3]
[2, 3, 7]
[2, 3, 7, 101]
[2, 3, 7, 18] => length = 4
```

---

## 🔁 Comparison Table

| Feature               | Approach 1 (DP)     | Approach 2 (Binary Search) |
|-----------------------|---------------------|-----------------------------|
| Time Complexity       | O(n²)               | O(n log n)                  |
| Space Complexity      | O(n)                | O(n)                        |
| Builds LIS?           | Partially           | No (only length)           |
| Intuition Level       | Simple to Understand| Tricky but Efficient        |
| Java Feature Used     | `Arrays.stream()`   | `List`, custom binarySearch|

---

## 🧠 Notes for Future Reference

- Use **DP** if LIS reconstruction is needed.
- Use **Binary Search** if performance is key.
- In binary search, `res` may not store the actual LIS, but it helps track length optimally.
- The custom `binarySearch()` method mimics `Collections.binarySearch()` but is tailored to return the **insertion index**.

---

Would you like me to generate a **flowchart image** to show how either approach works visually?

