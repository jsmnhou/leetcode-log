# 153. Find Minimum in Rotated Sorted Array

Date (of first attempt): 04/01/2024  
Difficulty: Medium  
Topics: Binary Search  
Question Link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/  

## Problem

```
Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

 

Example 1:

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
Example 2:

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
Example 3:

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```

## Solutions

### Binary Search

Go through lots of cases to identify what to do when `M > L`, `M < L`, and `M = L`.

**Cases**:
1 2 3
	L < R: return L
3 1 2
	M < L: move R to M (not M-1 since that would overshoot)
2 3 1
	M > L: move M to L+1 (next it., M=L)
2 1
	M = L: return R

```python
class Solution(object):
    def findMin(self, nums):
        if not nums: return None
        if len(nums) < 2: return nums[0]

        L, R = 0, len(nums) - 1
        
        while nums[L] > nums[R]:
            # Case L = R, ref. to same elt, return nums[L].
            # Case: L < R, return nums[L].
            
            M = (L + R) // 2
            if nums[M] > nums[L]:
                L = M+1
            elif nums[M] < nums[L]:
                R = M
            else:
                return nums[R]
        return nums[L]

# Time: O(N) where N is # elts in nums.
# Space = O(1) b/c only need 2 ptrs.

# when L > R (array has been rotated)
# if L < R, return L immediately (must be smallest)

# Case: L < R
    # return L directly

# Case: M > L(>R)
    # move L to M+1
# Case: M < L(>R)
    # move R to M
# Case: M = L(>R)
    # return R
```