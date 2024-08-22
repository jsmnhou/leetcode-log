# 347. Top K Frequent Elements

Date (of first attempt): 08/06/2024
Difficulty: Medium
Topics: Priority Queue
Quick Notes: Finding k “largest” —> use minPQ
Question Link: https://leetcode.com/problems/top-k-frequent-elements/description/
1D: No
2D: No
7D: No
15D: No
1M: No
3M: No
Days since first attempt: -16
Last edited time: August 6, 2024 3:49 PM

## Problem

```
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

 

Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
Example 2:

Input: nums = [1], k = 1
Output: [1]
```

```

```

## Solutions

### Priority Queue

If finding k “largest,” use minPQ. (Tried maxPQ at first, failed horribly)

```python
import heapq as hq

class Solution(object):
    def topKFrequent(self, nums, k):

        num_to_count = defaultdict(int)
        for num in nums:
            num_to_count[num] += 1

        minpq = []
        for num, count in num_to_count.items():
            to_add = (count, num)

            if len(minpq) == k:
                if count > minpq[0][0]:
                    hq.heappushpop(minpq, to_add)
            else:
                hq.heappush(minpq, to_add)
        
        return [num for (_, num) in minpq]

# pq
# dictionary: (number -> count)
# push (num, count) into minpq, sort ascending by (count)
# keep only k elts in minpq, only add num if num > curr_head

# Time: O(nlogk)
    # O(n) loop through nums/create dictionary
    # O(nlogk) loop through nums, each push O(logk)
# Space: O(n)
```