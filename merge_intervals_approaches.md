# 🧠 Merge Intervals – Two Approaches

## 📌 Problem Statement

Given an array of intervals `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example:**
```
Input:  [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

---

## 💡 Approach 1: Classic Sorting and Merging

### 🔍 Intuition

- Sort intervals by their start time.
- Iterate over them and check for overlap:
  - If the current interval overlaps with the previous one, **merge** them by extending the end time.
  - Otherwise, **add the interval** as a new entry.

This is the standard greedy technique and works efficiently with a time complexity of **O(n log n)** due to sorting.

---

### 💻 Code: Classic Merge
```java
public int[][] merge(int[][] intervals) {
    List<int[]> res = new ArrayList<>();
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    res.add(intervals[0]);

    for (int i = 1; i < intervals.length; i++) {
        if (res.get(res.size() - 1)[1] >= intervals[i][0]) {
            res.get(res.size() - 1)[1] = Math.max(intervals[i][1], res.get(res.size() - 1)[1]);
        } else {
            res.add(intervals[i]);
        }
    }

    return res.toArray(new int[0][0]);
}
```

---

## 💡 Approach 2: Sweep Line Algorithm

### 🔍 Intuition

> Treat every interval as two events on a timeline:
> - A `+1` at the start (an interval begins).
> - A `-1` at the end (an interval ends).

As you **sweep from left to right** (via a `TreeMap`), you track how many intervals are "active" at each moment using a counter:
- When the counter rises from `0 → 1`: a new merged interval **starts**.
- When it drops from `1 → 0`: the merged interval **ends**.

This avoids direct comparisons and naturally handles overlaps through counting.

---

### 💻 Code: Sweep Line
```java
public int[][] merge(int[][] intervals) {
    TreeMap<Integer, Integer> map = new TreeMap<>();
    
    for (int[] interval : intervals) {
        map.put(interval[0], map.getOrDefault(interval[0], 0) + 1);
        map.put(interval[1], map.getOrDefault(interval[1], 0) - 1);
    }

    List<int[]> res = new ArrayList<>();
    int have = 0;
    int[] interval = new int[2];

    for (int point : map.keySet()) {
        if (have == 0) interval[0] = point;
        have += map.get(point);
        if (have == 0) {
            interval[1] = point;
            res.add(new int[] {interval[0], interval[1]});
        }
    }

    return res.toArray(new int[res.size()][]);
}
```

---

## 🧮 Complexity Comparison

| Approach       | Time Complexity | Space Complexity |
|----------------|------------------|-------------------|
| Classic Merge  | O(n log n)       | O(n)              |
| Sweep Line     | O(n log n)       | O(n)              |

---

## ✅ Summary

- Use the **classic approach** when you want simplicity and readability.
- Use the **sweep line** approach to better understand event-based processing and timeline counting – very useful for advanced interval problems.