
# 📘 LeetCode 435: Non-overlapping Intervals

## 📝 Problem Statement

Given an array of intervals where `intervals[i] = [starti, endi]`, return the **minimum number of intervals you need to remove** to make the rest of the intervals **non-overlapping**.

---

## ✅ Example

```text
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: Remove [1,3] and the rest of the intervals are non-overlapping.
```

---

## 💡 Approach

This is a classic **Greedy Algorithm** problem.

### 🎯 Goal

We want to **keep the maximum number of non-overlapping intervals**, which is equivalent to removing the **minimum number of overlapping** ones.

### 🧠 Steps

1. **Sort** the intervals by their `end` time.
2. Initialize:
   - `end = -∞`: to track the end of the last non-overlapping interval
   - `count = 0`: to track the number of intervals we remove
3. Traverse each interval:
   - If `interval.start >= end`: ✅ no overlap → keep
     - Update `end = interval.end`
   - Else: ❌ overlap → remove this interval
     - Increment `count`

---

## 🧪 Test Case Walkthrough

**Input:** `[[1,2],[2,3],[3,4],[1,3]]`

1. **Sort by end time**:
   ```text
   [[1,2],[1,3],[2,3],[3,4]]
   ```
2. **Initial values**: `end = -∞`, `count = 0`
3. Iterate:
   - `[1,2]`: `1 >= -∞` → keep, `end = 2`
   - `[1,3]`: `1 < 2` → overlap, remove → `count = 1`
   - `[2,3]`: `2 >= 2` → keep, `end = 3`
   - `[3,4]`: `3 >= 3` → keep, `end = 4`

✅ Total removed = `1`

---

## ⏱️ Complexity

| Type             | Complexity        |
|------------------|-------------------|
| Time Complexity  | O(n log n) (for sorting) |
| Space Complexity | O(1) (in-place sort)    |

---

## 🧾 Code

### ✅ Python

```python
from typing import List

class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: x[1])
        end = float('-inf')
        count = 0

        for interval in intervals:
            if interval[0] >= end:
                end = interval[1]
            else:
                count += 1

        return count
```

---

---
## 💡 Approach 2

### 🔍 Your Java Approach Explained

1. **Sort intervals by end time** so that we always choose the interval that finishes earliest, maximizing room for future intervals.
2. Initialize `prev` as the end time of the first interval and `overLaps = 0` to count how many overlapping intervals we remove.
3. Loop through the rest of the intervals:
   - If the current interval’s start is **less than `prev`**, it overlaps → we **remove** it and increment `overLaps`.
   - If there's **no overlap**, update `prev` to the current interval’s end.
4. Return the total number of removed intervals `overLaps`.

> ✅ This greedy strategy ensures that we always keep the interval that allows more space for future ones, hence removing the least number of intervals.


This is a classic **Greedy Algorithm** problem.


```java
import java.util.Arrays;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int overLaps = 0;
        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);  // Sort by end time
        int prev = intervals[0][1];  // End time of the first interval

        for (int i = 1; i < intervals.length; i++) {
            if (prev > intervals[i][0]) {
                // Overlapping interval → remove
                overLaps++;
            } else {
                // No overlap → update end time tracker
                prev = intervals[i][1];
            }
        }

        return overLaps;
    }
}
```

---

## 📌 Key Takeaway

- This is a greedy optimization problem.
- Sorting by end time ensures we always have the most room left for future intervals.
