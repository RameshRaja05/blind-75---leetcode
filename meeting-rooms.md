Let's document the **"Meeting Rooms"** LeetCode problem with your solution, explanation, algorithm walkthrough, and visuals in a clear Markdown format. This typically refers to **LeetCode 252: Meeting Rooms**.

---

## 📄 Markdown Documentation: LeetCode 252 - Meeting Rooms

### Problem Statement

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]`, determine if a person could attend all meetings.

### Example

**Input:**

```java
intervals = [[0,30],[5,10],[15,20]]
```

**Output:**

```
false
```

**Explanation:**
The meetings `[0,30]` and `[5,10]` overlap, so a single person cannot attend both.

---

### Constraints

* `0 <= intervals.length <= 10^4`
* `intervals[i].length == 2`
* `0 <= starti < endi <= 10^6`

---

## ✅ Approach: Sorting Intervals by Start Time

To check for overlapping meetings:

1. **Sort** the intervals based on the start time.
2. **Iterate** through the sorted list and check if any current meeting starts before the previous one ends.
3. If any such overlap is found, return `false`. Otherwise, return `true`.

---

## 🔍 Algorithm Walkthrough (Step-by-Step)

* **Input:** `[[0,30],[5,10],[15,20]]`
* **Step 1:** Sort intervals → `[[0,30],[5,10],[15,20]]`
* **Step 2:** Compare intervals:

  * Compare `[0,30]` and `[5,10]` → 5 < 30 → Overlap ❌
* **Output:** `false`

---

## 🧠 Intuition

* If any meeting starts **before** the previous meeting ends, they overlap.
* Sorting ensures we only need to check **consecutive** intervals.

---

## 💻 Code (Java)

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]); // sort by start time

        for (int i = 1; i < intervals.length; i++) {
            // check for overlap
            if (intervals[i][0] < intervals[i - 1][1]) {
                return false;
            }
        }

        return true;
    }
}
```

---

## ⏱️ Time and Space Complexity

* **Time Complexity:** `O(N log N)` for sorting
* **Space Complexity:** `O(1)` (in-place comparison)

---

## 📊 Visual Example

**Before Sorting:**

```
[0,30]
[5,10]
[15,20]
```

**After Sorting:**

```
[0,30]  → Ends at 30  
[5,10]  → Starts at 5 → ❌ Overlap with 30  
```

---

Would you like me to document the follow-up version (LeetCode 253: Meeting Rooms II) where we calculate the **minimum number of meeting rooms required**?
