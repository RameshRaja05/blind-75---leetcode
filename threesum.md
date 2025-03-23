**Problem Statement:**
Given an integer array `nums`, return all unique triplets `[nums[i], nums[j], nums[k]]` such that:
- `i != j`, `i != k`, and `j != k`
- `nums[i] + nums[j] + nums[k] == 0`

**Constraints:**
- `3 <= nums.length <= 3000`
- `-10^5 <= nums[i] <= 10^5`

The solution set must not contain duplicate triplets.

---

**Example 1:**
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation:
The distinct triplets that sum up to zero are:
[-1, -1, 2] and [-1, 0, 1]
```

**Example 2:**
```
Input: nums = [0,1,1]
Output: []
Explanation: No valid triplet sums to zero.
```

**Example 3:**
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums to zero.
```

---

**Approach:**
1. **Sorting:** First, sort the array to simplify the duplicate removal process and enable a two-pointer approach.
2. **Iterate through the array:** Fix one element (`nums[i]`) and use two pointers (`left` and `right`) to find pairs that sum to `-nums[i]`.
3. **Two-pointer technique:** Move `left` and `right` pointers to find valid triplets efficiently.
4. **Skip duplicates:** Avoid duplicate triplets by skipping repeated elements after finding a valid triplet.

This approach ensures an optimal solution with **O(n²) time complexity** and **O(1) extra space complexity**.

---

**Java Solution:**
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        
        for(int i = 0; i < nums.length; i++){
           if(i > 0 && nums[i] == nums[i - 1])
               continue; // Skip duplicates
            
           int left = i + 1;
           int right = nums.length - 1;
           
            while(left < right){
               int threeSum = nums[i] + nums[left] + nums[right];
               if(threeSum < 0)
                   left++;
               else if(threeSum > 0)
                   right--;
               else {
                   res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                   left++;
                   right--;
                   while(left < right && nums[left] == nums[left - 1])
                       left++; // Skip duplicates
               }
            }
        }
        return res;
    }
}
```

---

**Time Complexity:**
- **O(n²)**: The outer loop runs `O(n)`, and the two-pointer approach runs in `O(n)` for each iteration.

**Space Complexity:**
- **O(1)**: No extra space is used apart from the result list.

This method efficiently finds unique triplets that sum to zero while avoiding duplicate results.

