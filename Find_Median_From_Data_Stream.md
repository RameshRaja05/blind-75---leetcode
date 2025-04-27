
# LeetCode: 295. Find Median from Data Stream

## 🧩 Problem Statement

Design a data structure that supports the following operations efficiently:

- `addNum(int num)`: Add a number from the data stream to the data structure.
- `findMedian()`: Return the median of all elements so far.

Implement the `MedianFinder` class:
```java
MedianFinder() // Initializes the object
void addNum(int num) // Adds num to the data stream
double findMedian() // Returns the median of all elements
```

---

## 🔍 Intuition

To efficiently track the median of a stream of numbers:
- Use two heaps:
  - A **Max Heap** (`smallHeap`) for the **smaller half** of numbers
  - A **Min Heap** (`largeHeap`) for the **larger half** of numbers

By maintaining balance between these two heaps:
- If the total number of elements is **even**, the median is the average of the two heap tops.
- If **odd**, the median is the top of the larger heap.

---

## 📊 Visual Example

Let’s say the stream is: `5 → 15 → 1 → 3`

After each insertion:

| Step | Number | smallHeap (max-heap) | largeHeap (min-heap) | Median |
|------|--------|------------------------|------------------------|--------|
| 1    | 5      | [5]                   | []                    | 5      |
| 2    | 15     | [5]                   | [15]                  | (5+15)/2 = 10 |
| 3    | 1      | [5, 1]                | [15]                  | 5      |
| 4    | 3      | [3, 1]                | [5, 15]               | (3+5)/2 = 4 |

---

## 🧠 Algorithm Steps (Walkthrough)

1. **Initialize** two priority queues:
   - `smallHeap` = max heap (`Collections.reverseOrder()`)
   - `largeHeap` = min heap

2. **Add Number (`addNum`)**
   - Add the number to `smallHeap`.
   - Ensure that `smallHeap.peek() <= largeHeap.peek()` by moving the largest from `smallHeap` to `largeHeap` if necessary.
   - Balance heap sizes if difference > 1 by moving elements between heaps.

3. **Find Median (`findMedian`)**
   - If one heap has more elements → return its top.
   - If equal size → return average of the tops.

---

## 🧑‍💻 Code

```java
class MedianFinder {
    // Max heap for smaller half of numbers
    Queue<Integer> smallHeap;
    // Min heap for larger half of numbers
    Queue<Integer> largeHeap;

    public MedianFinder() {
        smallHeap = new PriorityQueue<>(Collections.reverseOrder());
        largeHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        // Step 1: Add to max heap (smallHeap)
        smallHeap.offer(num);

        // Step 2: Maintain order — maxHeap top should be <= minHeap top
        if (!smallHeap.isEmpty() && !largeHeap.isEmpty() && smallHeap.peek() > largeHeap.peek()) {
            largeHeap.offer(smallHeap.poll());
        }

        // Step 3: Balance the sizes of both heaps
        if (largeHeap.size() > smallHeap.size() + 1) {
            smallHeap.offer(largeHeap.poll());
        } else if (smallHeap.size() > largeHeap.size() + 1) {
            largeHeap.offer(smallHeap.poll());
        }
    }

    public double findMedian() {
        if (largeHeap.size() > smallHeap.size()) {
            return largeHeap.peek();
        } else if (smallHeap.size() > largeHeap.size()) {
            return smallHeap.peek();
        } else {
            return (smallHeap.peek() + largeHeap.peek()) / 2.0;
        }
    }
}
```

---

## ⏱️ Time & Space Complexity

| Operation      | Time Complexity | Space Complexity |
|----------------|------------------|------------------|
| `addNum()`     | O(log n)         | O(n)             |
| `findMedian()` | O(1)             | O(1)             |

- `addNum` inserts and balances heaps: `O(log n)` due to heap insertions.
- `findMedian` is `O(1)` since it peeks at the tops.
- Space grows linearly with number of elements.

---

## 🖼️ Optional Diagram Suggestions

You can visualize this with:
- A **heap diagram** showing elements in `smallHeap` and `largeHeap`
- A **flowchart** for logic in `addNum()` to understand heap balancing

Let me know if you'd like me to generate those as images!
