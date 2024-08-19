# 133. Clone Graph

Date (of first attempt): 04/17/2024
Difficulty: Medium
Topics: BFS, DFS
Quick Notes: Using function itself as recursive function.
Question Link: https://leetcode.com/problems/clone-graph/description/
1D: Yes
2D: No
7D: No
15D: No
1M: No
3M: No
Days since first attempt: -123
Last edited time: April 20, 2024 11:24 PM

## Problem

```
Given a reference of a node in a connected undirected graph.

Return a **deep copy (clone)** of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

class Node {
    public int val;
    public List<Node> neighbors;
}
 

Test case format:

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

 

Example 1:

Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

![Untitled](133%20Clone%20Graph%20206938b8750742718d51712b2030a7cf/Untitled.png)

```

```

## Solutions

### DFS

```python
class Solution(object):
    def __init__(self):
        self.visited = {} # real A -> cloned A'

    def cloneGraph(self, node):
        if not node:
            return None
        
        if node in self.visited: 
            return self.visited[node]
        
        cloned_node = Node(node.val)
        self.visited[node] = cloned_node

        for nei in node.neighbors:
            cloned_node.neighbors.append(self.cloneGraph(nei))

        return cloned_node
        
# Time: O(N + M) where N is number of nodes, M is number of edges
	# We traverse every node and edge exactly once.
# Space: O(N)
	# Size of visited dictionary
	# Also O(H) where H is size of recursive stack
```

### BFS

```python
class Solution(object):

    def cloneGraph(self, node):
        if not node:
            return None
        
        visited = {}
        res = parent_clone = Node(node.val)
        visited[node] = parent_clone
        queue = []
        for child in node.neighbors:
            queue.append((parent_clone, child))
        
        while queue:
            parent_clone, child = queue.pop(0)
            if child in visited:
                parent_clone.neighbors.append(visited[child])
                continue
            
            child_clone = Node(child.val)
            visited[child] = child_clone
            parent_clone.neighbors.append(child_clone)
            for c in child.neighbors:
                queue.append((child_clone, c))
        
        return res
        
# Time: O(N + M)
# Space: O(N)
	# visited, queue
```