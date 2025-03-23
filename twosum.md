**Two Sum**
**Problem Statement:**
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`.

**Constraints:**
- Each input will have exactly one solution.
- The same element cannot be used twice.
- The answer can be returned in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

---

**Approach:**
To solve this problem efficiently, we use a HashMap to store the numbers we have seen so far along with their corresponding indices. By doing this, we can check in constant time whether the complement of the current number (i.e., `target - nums[i]`) exists in the map. If it does, we return the stored index and the current index.

**Algorithm:**
1. Initialize a HashMap to store numbers and their indices.
2. Iterate through the array:
   - Check if the current number exists in the HashMap (i.e., a complement was previously stored).
   - If it exists, return the stored index and the current index.
   - Otherwise, store `target - nums[i]` as the key and `i` as the value in the HashMap.
3. If no solution is found, return an empty array (though, per constraints, there will always be one solution).

---

**Java Solution:**
```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                return new int[]{map.get(nums[i]), i};
            }
            map.put(target - nums[i], i);
        }
        return new int[]{}; // This will never be reached as per problem constraints
    }
}
```
**Python Solution:**
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap={}
        for i in range(len(nums)):
            diff=target-nums[i]
            if diff in hashmap:
                return [hashmap[diff],i]
            hashmap[nums[i]]=i
        return []
```   

---

**Time Complexity:**
- **O(n)**: We traverse the array once, and HashMap operations (insertion and lookup) take O(1) on average.

**Space Complexity:**
- **O(n)**: In the worst case, we store all elements in the HashMap.

This approach provides an optimal solution to the problem using a single pass through the array.

