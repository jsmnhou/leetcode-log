# 207. Course Schedule

Date (of first attempt): 04/13/2024
Difficulty: Medium
Topics: DFS, Topological Sort
Quick Notes: Topological sort: push in nodes w/ no incoming edges. Visit their children, push in child if no more incoming edges after removing parent. If numVisited < numNodes, cycle was detected (not all nodes traversed).
Question Link: https://leetcode.com/problems/course-schedule/description/
1D: Yes
2D: No
7D: No
15D: No
1M: No
3M: No
Days since first attempt: -117
Last edited time: April 15, 2024 10:58 PM

## Problem

```
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

```

```

## Solutions

### Topological Sort

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        incoming = [0] * numCourses
        graph = defaultdict(list) # node -> next_node(s)

        for target, pre in prerequisites:
            graph[pre].append(target) # pre -> target
            incoming[target] += 1
        
        queue = []
        visited = 0
        for i, count in enumerate(incoming):
            if count == 0:
                queue.append(i)

        while queue:
            curr = queue.pop(0)
            visited += 1
            for nei in graph[curr]:
                incoming[nei] -= 1
                if incoming[nei] == 0:
                    queue.append(nei)

        return not visited < numCourses

# Time: O(m + n) where n = # of courses, m = size of prereq array
    # O(m) to create graph
    # O(n) to add initial nodes to queue
    # while loop iterates through all edges exactly once
        # m edges total -> O(m)
# Space: O(m + n)
    # O(m) for graph, O(n) for incoming, O(n) worst case for queue
```

### DFS

Cycle is detected if we find the same node again within the current DFS stack. If the current node isn’t w/in the current stack but was already visited, this is good because that means no cycle was detected (return True). 

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        adj = defaultdict(list)

        for target, pre in prerequisites:
            adj[pre].append(target)
        
        visited = [False] * numCourses
        inStack = [False] * numCourses
        for i in range(numCourses):
            if self.dfs(i, adj, visited, inStack): # cycle found
                return False
        return True
        
    
    def dfs(self, node, adj, visited, inStack):
        if inStack[node]: # cycle found
            return True
        if visited[node]: # already visited, didn't find cycle
            return False
        visited[node], inStack[node] = True, True
        
        for nei in adj[node]:
            if self.dfs(nei, adj, visited, inStack): # cycle found
                return True
        
        # remove curr node from the dfs stack
        inStack[node] = False
        return False

# Time: O(m + n)
    # O(m) to make the graph
    # O(n) to init visited and inStack
    # Total calls to dfs function will traverse all the edges (m)
# Space: O(m + n)
    # Worst case dfs stack goes up to n (total courses)
```