**Problem Statement:**
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. The answer can be returned in any order.

**Example 1:**
Input: `nums = [1,1,1,2,2,3]`, `k = 2`
Output: `[1,2]`

**Example 2:**
Input: `nums = [1]`, `k = 1`
Output: `[1]`

**Constraints:**
- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `k` is in the range `[1, the number of unique elements in the array]`.
- The answer is guaranteed to be unique.

**Follow-up Requirement:**
The algorithm's time complexity must be better than `O(n log n)`.

---

## Approach:
1. **Use a Hash Map to Count Frequencies:**
   - Traverse the `nums` array and store the frequency of each number in a hash map (dictionary in Python).
   - This step takes `O(n)` time.

2. **Use a Min-Heap to Find the `k` Most Frequent Elements:**
   - A min-heap (priority queue) is used to keep track of the top `k` frequent elements.
   - Push elements into the heap based on their frequency.
   - Since a min-heap maintains the smallest element at the top, we can efficiently keep the top `k` frequent elements.
   - The heap operations (`push` and `pop`) take `O(log k)`, and for `n` elements, this results in `O(n log k)` time complexity.

3. **Extract Elements from the Heap:**
   - Extract the elements from the heap to get the `k` most frequent numbers.
   - This step takes `O(k log k)`, which is generally much smaller than `O(n log n)`.

**Time Complexity Analysis:**
- Constructing the frequency map: `O(n)`
- Heap operations: `O(n log k)`
- Extracting elements: `O(k log k)`
- **Overall Complexity:** `O(n log k)` (which is better than `O(n log n)`).

---

## Solution (Python Implementation):
```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    # Step 1: Count frequencies using Counter
    freq_map = Counter(nums)
    
    # Step 2: Use a min-heap to keep track of top k elements
    heap = []
    for num, freq in freq_map.items():
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:
            heapq.heappop(heap)  # Remove the least frequent element
    
    # Step 3: Extract elements from the heap
    return [num for freq, num in heap]
```

---

## Alternative Approach (Using Bucket Sort for `O(n)` Complexity):
1. **Count frequencies:** Use a hash map to store element frequencies.
2. **Use an array (bucket) where the index represents the frequency.**
   - Place numbers in their corresponding frequency index.
   - The highest frequencies will be at the end of the array.
3. **Extract the top `k` elements by iterating from the highest frequency index backward.**

**Time Complexity:** `O(n)`, since we only use frequency counting and bucket placement.

```python
from collections import Counter

def topKFrequent(nums, k):
    freq_map = Counter(nums)
    bucket = [[] for _ in range(len(nums) + 1)]
    
    for num, freq in freq_map.items():
        bucket[freq].append(num)
    
    result = []
    for i in range(len(bucket) - 1, 0, -1):
        for num in bucket[i]:
            result.append(num)
            if len(result) == k:
                return result
```

---

## Conclusion:
- **Heap-based solution (`O(n log k)`)** works well for large `n` but small `k`.
- **Bucket sort solution (`O(n)`)** is optimal for cases where the maximum frequency does not exceed `n`.

