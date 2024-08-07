# 435. Non-overlapping Intervals

Date (of first attempt): 04/10/2024
Difficulty: Medium
Topics: Greedy, Intervals
Quick Notes: W/ intervals, often try sorting by end time. We we need to choose one or the other, try greedy (see if choosing “a” Is always AT LEAST AS GOOD AS (≥) choosing “b”).
Question Link: https://leetcode.com/problems/non-overlapping-intervals/description/
1D: Yes
2D: Yes
7D: No
15D: No
1M: No
3M: No
Days since first attempt: -119
Last edited time: April 11, 2024 12:20 PM

## Problem

```
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

 

Example 1:

Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
Example 2:

Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
Example 3:

Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping
```

```

```

## Solutions

### Greedy

Intuition: 

Choosing min # of intervals to make everything not overlap is same as **choosing max # of non-overlapping intervals**.

Say we have this conflict:

[1, 4] & [3, 5]

a_start < b_start < a_end < b_end

We must choose between the two. If we choose to keep “a”, we have more potential intervals to choose from down the line. If we choose to keep “b”, we may lose access to those intervals. Choosing “b” will never give us a better answer than choosing “a”.

Num_choices with “a” is always ≥ num_choices with “b”. Therefore, we should always greedily choose “a”, aka the conflicting interval with the lesser end time. 

Keep track of the “latest_end” time to compare each interval’s start time to. If we identify a conflict, keep “latest_end” the same as we are taking the prev interval (”a”). If there is no conflict, update “latest_end.”

```python
class Solution(object):
    def eraseOverlapIntervals(self, intervals):
        intervals.sort(key=lambda x:x[1]) # sort by end times
        res = 0
        latest_end = float('-inf')

        for start, end in intervals:
            if start < latest_end: # conflict; take prev
                res += 1
            else: # no conflict, update latest_end
                latest_end = end
        
        return res

# Time: O(NlogN) to sort intervals
# Space: technically O(N) b/c of python sort, but ignoring that O(1)
```