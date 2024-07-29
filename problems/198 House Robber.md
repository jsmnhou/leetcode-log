# 198. House Robber

Date (of first attempt): 03/30/2024  
Difficulty: Medium  
Topics: 1-D Dynamic Progamming  
Quick Notes: Because we only use memo[i-1] and memo[i-2], can use 2 vars approach.  
Question Link: https://leetcode.com/problems/house-robber/description/  

## Problem

```
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

## Solutions

### Dynamic Programming (Bottom-Up)

```python
class Solution(object):
    def rob(self, nums):
        if not nums: return 0
        if len(nums) < 2: return nums[0]
        
        memo = [0] * len(nums)
        memo[0] = nums[0]
        memo[1] = max(nums[0], nums[1])

        for i in range(2, len(nums)):
            memo[i] = max(memo[i-1], memo[i-2] + nums[i])

        return memo[len(nums) - 1]

# Time: O(N) since iterate from 2 to N-1
# Space: O(N) for the memo

# can't adjacent houses
# get max amt of money

# recurrence relation:
# F(i) = max amt possible up until index i.
# Case 1: we can take current house (can't take prev adjacent)
    # sum current money w/ F(i-2)
# Case 2: we can take prev adjacent
    # return F(i-1)
# take max of the two

# base case:
# If you have 1 house only, return that money.
# If you have 2 houses, return max of the 2.

# return F(len(nums) - 1)
```

### Optimized DP

```python
class Solution(object):
    def rob(self, nums):
        if not nums: return 0
        p1, p2 = 0, 0

        for num in nums:
            tmp = p1
            p1 = max(p2 + num, p1)
            p2 = tmp
        
        return p1

# Time: O(N) same.
# Space: O(1), only need 2 variables instead of entire memo.

# p1=max(...,...) ; this p1's value = current best (after this it.)
# p1=max(... ,p1) ; old p1's value = current best 1 it. ago
# p1=max(p2+num ,p1) ; old p2's value = current best 2 it. ago
    # update p2 to current best 1 it. ago (for next it.)
    # p2 <- tmp
```