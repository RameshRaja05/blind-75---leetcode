**Problem Statement:**
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

**Examples:**

Example 1:
```
Input: nums = [1,2,3,1]
Output: true
```
Explanation: The element `1` appears twice at indices `0` and `3`.

Example 2:
```
Input: nums = [1,2,3,4]
Output: false
```
Explanation: All elements are distinct.

Example 3:
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

**Constraints:**
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

---

## **Approach:**
### **1. Using HashSet (Efficient Approach)**
**Idea:** We can use a HashSet to store elements while iterating through the array. If an element is already present in the set, return `true`. Otherwise, add it to the set.

**Algorithm:**
1. Initialize an empty HashSet.
2. Iterate through each element in `nums`:
   - If the element exists in the set, return `true`.
   - Otherwise, add it to the set.
3. If the loop completes, return `false`.

**Code Implementation (Java):**
```java
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (set.contains(num)) {
                return true;
            }
            set.add(num);
        }
        return false;
    }
}
```

**Time Complexity:** `O(n)` (In the worst case, we traverse all elements once and perform `O(1)` operations per element.)
**Space Complexity:** `O(n)` (In the worst case, we store all `n` elements in the HashSet.)

---

### **2. Sorting Approach**
**Idea:**
If we sort the array, any duplicate elements will be adjacent. We can iterate once and check if consecutive elements are equal.

**Algorithm:**
1. Sort the array.
2. Iterate through the sorted array and check if `nums[i] == nums[i + 1]`.
3. If a duplicate is found, return `true`.
4. If no duplicates are found, return `false`.

**Code Implementation (Java):**
```java
import java.util.Arrays;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}
```

**Time Complexity:** `O(n log n)` (Sorting takes `O(n log n)`, and checking for duplicates takes `O(n)`, so overall `O(n log n)`).
**Space Complexity:** `O(1)` (If sorting is done in place, otherwise `O(n)` for sorting algorithms that use extra space.)

---

### **3. Brute Force Approach (Not Recommended)**
**Idea:** Compare each element with every other element.

**Algorithm:**
1. Use a nested loop to compare each element with all other elements.
2. If a duplicate is found, return `true`.
3. If the loop completes, return `false`.

**Code Implementation (Java):**
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

**Time Complexity:** `O(n^2)` (Nested loop for all pairs)
**Space Complexity:** `O(1)`

---

## **Best Approach:**
- The **HashSet approach** is the best for most cases (`O(n)` time, `O(n)` space).
- The **Sorting approach** is also good if modifying the array is allowed (`O(n log n)` time, `O(1)` space).
- The **Brute force approach** is inefficient for large inputs (`O(n^2)`).

### **Conclusion:**
If space is not a constraint, use **HashSet**. If you must use `O(1)` space, use **Sorting**.

