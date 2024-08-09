# 152. Maximum Product Subarray

Date (of first attempt): 04/15/2024  
Difficulty: Medium  
Topics: 1-D Dynamic Progamming  
Quick Notes: DP with no array needed; annoying cases with negative numbers handled w/ updating “min_so_far” var.  
Question Link: https://leetcode.com/problems/maximum-product-subarray/description/  

## Problem

```
Given an integer array nums, find a 
subarray
 that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

 

Example 1:

Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
Example 2:

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## Solutions

### Dynamic Programming

Annoying cases to consider:

- There’s a 0
- Negative numbers

Solution is to maintain 2 running vars, “max/min_so_far”. 

Handling 0’s:

“max/min_so_far” by construction MUST include nums[i], so if nums[i]=0, then both would be set to 0 (as needed).

Handling negatives:

When we multiply “min_so_far” with current nums[i], if “min_so_far” had been negative and now nums[i] is also negative, this makes their product a candidate for final_max (since it’s positive). This product is under consideration for being the updated “max_so_far” and thus will be in consideration for final_max (res). 

```python
class Solution(object):
    def maxProduct(self, nums):
        if not nums: return

        res = nums[0]
        min_so_far = max_so_far = nums[0]

        for i in range(1, len(nums)):
            p1 = nums[i]
            p2 = max_so_far * nums[i]
            p3 = min_so_far * nums[i]

            max_so_far = max(max(p1, p2), p3)
            min_so_far = min(min(p1, p2), p3)
            res = max(res, max_so_far)
        
        return res

# Time: O(N) where N is size of nums
# Space: O(1)
```