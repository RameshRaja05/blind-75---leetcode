**Problem Statement:**
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

**Example:**
```
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4].
```

---

## **Approach Using Sorting**

### **Steps to Solve the Problem:**
1. **Sort the Array:**
   - Sorting the array will allow us to process numbers in order, making it easier to identify consecutive sequences.
   - Sorting takes **O(n log n)** time.

2. **Iterate Through the Sorted Array:**
   - Track the length of the current consecutive sequence.
   - If two adjacent numbers are the same, ignore the duplicate.
   - If the current number is exactly **one greater** than the previous number, increment the streak count.
   - Otherwise, update the longest streak and reset the count.

3. **Return the Longest Streak:**
   - Ensure the last tracked streak is considered before returning the result.

### **Code Implementation:**
```python
def longestConsecutive(nums):
    if not nums:
        return 0

    nums.sort()  # Step 1: Sort the array
    longest_streak = 1
    current_streak = 1

    for i in range(1, len(nums)):  # Step 2: Iterate through sorted nums
        if nums[i] == nums[i - 1]:  # Skip duplicates
            continue
        if nums[i] == nums[i - 1] + 1:  # Check if it's consecutive
            current_streak += 1
        else:
            longest_streak = max(longest_streak, current_streak)
            current_streak = 1  # Reset for the next sequence

    return max(longest_streak, current_streak)
```

### **Complexity Analysis:**
- **Time Complexity:** `O(n log n)` (due to sorting)
- **Space Complexity:** `O(1)` (if sorting is done in place)

---

## **Alternative Approach: Using HashSet**
This approach improves efficiency by utilizing a HashSet to store elements and check sequence continuity in `O(1)` time.

### **Steps:**
1. **Insert all elements into a HashSet** to allow constant-time lookups.
2. **Find the start of a sequence** (a number `num` such that `num - 1` is not in the set).
3. **Expand the sequence** by checking consecutive numbers.
4. **Keep track of the longest sequence** and return it.

### **Code Implementation:**
```python
def longestConsecutive(nums):
    if not nums:
        return 0

    num_set = set(nums)
    longest_streak = 0

    for num in num_set:
        if num - 1 not in num_set:  # Check if it's the start of a sequence
            current_num = num
            current_streak = 1

            while current_num + 1 in num_set:
                current_num += 1
                current_streak += 1

            longest_streak = max(longest_streak, current_streak)

    return longest_streak
```

### **Complexity Analysis:**
- **Time Complexity:** `O(n)` (each number is processed once)
- **Space Complexity:** `O(n)` (using a HashSet)

---

## **Optimized Approach: Using HashMap (Union-Find)**
This approach optimizes the process by dynamically updating sequence lengths using a HashMap.

### **Steps:**
1. **Use a HashMap to store sequence lengths**, where:
   - The key is a number in `nums`.
   - The value is the length of the sequence it belongs to.
2. **Iterate through the array**:
   - If `num` is already in the map, skip it.
   - Otherwise, check for consecutive neighbors (`num - 1` and `num + 1`).
3. **Compute the sequence length (`sum`)**:
   - `sum = left + right + 1`, where `left` and `right` are sequence lengths.
4. **Update the HashMap**:
   - Update the sequence boundaries (start and end points) with the total sequence length.
5. **Track the maximum sequence length**.

### **Code Implementation:**
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Map<Integer,Integer> map=new HashMap<>();
        int maxLength=0;

        for(int num:nums){
            if(!map.containsKey(num)){
                int left=map.getOrDefault(num-1,0);
                int right=map.getOrDefault(num+1,0);

                int sum = left+right+1;
                maxLength=Math.max(maxLength,sum);

                if( left > 0 ) map.put(num-left,sum);
                if( right > 0 ) map.put(num+right,sum);

                map.put(num,sum);
            }
        }

        return maxLength;
    }
}
```

### **Complexity Analysis:**
- **Time Complexity:** `O(n)` (each number is processed once and updated efficiently in the HashMap)
- **Space Complexity:** `O(n)` (HashMap stores sequence boundaries)

---

## **Comparison of Approaches**
| Approach | Time Complexity | Space Complexity | Notes |
|----------|----------------|------------------|-------|
| Sorting | `O(n log n)` | `O(1)` | Simpler but slower |
| HashSet (Optimal) | `O(n)` | `O(n)` | Best for large inputs |
| HashMap (Union-Find) | `O(n)` | `O(n)` | Efficient but complex |

### **Conclusion:**
- If the input size is **small or already sorted**, the **sorting approach** works fine.
- If the input size is **large and unsorted**, the **HashSet approach** is recommended due to its `O(n)` time complexity.
- The **HashMap approach (Union-Find)** offers a more **dynamic and optimized** solution by keeping track of sequence lengths efficiently.


