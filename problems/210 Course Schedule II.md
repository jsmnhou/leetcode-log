# 210. Course Schedule II

Date (of first attempt): 04/14/2024  
Difficulty: Medium  
Topics: Topological Sort  
Quick Notes: See code to generate a valid topological sort order.  
Question Link: https://leetcode.com/problems/course-schedule-ii/description/  

## Problem

```
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
```

## Solutions

### Topological Sort

```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        adj = defaultdict(list)
        incoming = [0] * numCourses
        for target, pre in prerequisites:
            adj[pre].append(target) # pre -> target
            incoming[target] += 1
        
        queue = [] # node, preceding path
        for i in range(numCourses):
            if incoming[i] == 0:
                queue.append(i)
        
        visited = 0
        res = []
        while queue:
            curr = queue.pop(0)
            res.append(curr)
            visited += 1
            for nei in adj[curr]:
                incoming[nei] -= 1
                if incoming[nei] == 0:
                    queue.append(nei)
        
        if visited < numCourses:
            return []
        
        return res
        
# Time: O(E + V)
    # O(E) to build adj
    # O(V) to build inDegree array
    # intuitively, can also recognize that all nodes and edges
    # are processed exactly once.
# Space: O(E + V)
    # adj list, incoming array
```