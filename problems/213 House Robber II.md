# 213. House Robber II

Date (of first attempt): 03/30/2024  
Difficulty: Medium  
Topics: 1-D Dynamic Progamming  
Quick Notes: Same as first one, but now 2 cases.  
Question Link: https://leetcode.com/problems/house-robber-ii/description/  

## Problem

Houses arranged in a circle.

```
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
Example 2:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 3:

Input: nums = [1,2,3]
Output: 3

```

## Solution

### Optimized DP

See `198. House Robber`

```python
class Solution(object):
    def rob(self, nums):
        if not nums: return 0
        if len(nums) == 1: return nums[0]

        case1 = self.rob_simple(nums[1:])
        case2 = self.rob_simple(nums[:-1])
        return max(case1, case2)
    
    def rob_simple(self, nums):
        if not nums: return 0
        p1, p2 = 0, 0

        for num in nums:
            tmp = p1
            p1 = max(p2 + num, p1)
            p2 = tmp
        
        return p1

# Time: O(2N) = O(N)
# Space: O(1)

# 2 sub-problems:
    # consider houses 1 to N-1 (can't take 0th)
    # consider houses 0 to N-2 (can't take last)
# return better case
```