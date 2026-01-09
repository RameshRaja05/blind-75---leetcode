# 🧠 Problem: Insert Interval

**LeetCode Link:** [Insert Interval](https://leetcode.com/problems/insert-interval/)
**Difficulty:** Medium
**Tag(s):** Array, Sorting

---

## 📄 Problem Statement

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]`, sorted in ascending order by start time. You are also given a new interval `newInterval = [start, end]` that needs to be inserted into the list.

Insert `newInterval` into `intervals` such that the resulting list is still sorted in order and does not have any overlapping intervals (merge overlapping intervals if necessary).

Return the new list of intervals.

---

## 🧾 Example

### Example 1:

**Input:**

```
intervals = [[1,3],[6,9]]
newInterval = [2,5]
```

**Output:**

```
[[1,5],[6,9]]
```

### Example 2:

**Input:**

```
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [4,8]
```

**Output:**

```
[[1,2],[3,10],[12,16]]
```

---

## 🖼️ Visual Walkthrough

### Example:

* Intervals: `[[1,2], [3,5], [6,7], [8,10], [12,16]]`
* New Interval: `[4,8]`

**Visual Merge Flow:**

```
Original:
[1,2]   [3,5]   [6,7]   [8,10]   [12,16]
         |      |       |
         |<---- MERGE with [4,8] ---->|
         
Resulting Merge:
[1,2]   [3,10]   [12,16]
```

---

## 🧩 Approach

We follow a **3-phase** merge strategy:

1. **Add all intervals that end before `newInterval` starts.**
2. **Merge overlapping intervals with `newInterval`.**
3. **Add all intervals that start after `newInterval` ends.**

---

## 🧮 Algorithm Walkthrough (Step-by-step)

1. Initialize an empty result list.
2. Iterate through `intervals`:

   * If `interval.end < newInterval.start`: Add interval to result.
   * If `interval.start > newInterval.end`: Add `newInterval` to result (only once), then all remaining intervals.
   * Else: Overlap → merge by updating newInterval's start to `min(start)` and end to `max(end)`.
3. If `newInterval` hasn’t been added, append it.

---

## 🧑‍💻 Code (Python)

```python
def insert(intervals, newInterval):
    result = []
    i = 0
    n = len(intervals)

    # Phase 1: Before newInterval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1

    # Phase 2: Overlapping intervals
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    result.append(newInterval)

    # Phase 3: After newInterval
    while i < n:
        result.append(intervals[i])
        i += 1

    return result
```

## code (Java)

```
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> res = new ArrayList<>();
        boolean add = true;
        // three conditions
        // 1. new interval need to occur before current interval
        //     newinterval end value is less than current interval start value
        // 2. new interval need to occur after current interval
        //   newinterval start value is greater than current interval end value
        // 3. newinterval overlapping with current interval 
        // form a newinterval by taking minimum value of new and current interval start value
        // and maximum of new and current intervals end values

        for(int i = 0;i < intervals.length;i++){
            // first case
            if(newInterval[1] < intervals[i][0]){
                res.add(newInterval);
                while(i < intervals.length){
                    res.add(intervals[i]);
                    i++;
                }
                add = false;
                break;
            }else if(newInterval[0] > intervals[i][1]){
                res.add(intervals[i]);
            }else{
                newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
                newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            }
        }

        if(add){
          res.add(newInterval);
        }


        return res.toArray(new int[res.size()][2]);
    }
}

```

---

## ⏱️ Time & Space Complexity

| Complexity | Value                                     |
| ---------- | ----------------------------------------- |
| ⏰ Time     | O(n) – iterate through each interval once |
| 🧠 Space   | O(n) – output list can hold all intervals |

---

## ✅ Edge Cases

* `intervals` is empty → return `[newInterval]`.
* `newInterval` does not overlap → just insert it in the right place.
* `newInterval` overlaps all intervals → return the merged single interval.
