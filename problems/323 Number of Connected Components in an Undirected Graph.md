# 323. Number of Connected Components in an Undirected Graph

Date (of first attempt): 04/11/2024  
Difficulty: Medium  
Topics: DFS, Union Find  
Quick Notes: Basic Union Find review.  
Question Link: https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/  

## Problem

```
You have a graph of n nodes. You are given an integer n and an array edges where edges[i] = [ai, bi] indicates that there is an edge between ai and bi in the graph.

Return the number of connected components in the graph.

 

Example 1:

Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

![Untitled](323%20Number%20of%20Connected%20Components%20in%20an%20Undirecte%20b4af035eb8e243afb073762419dad9fb/Untitled.png)


## Solutions

### Disjoint Set Union (DSU)

See: [Union Find](https://www.notion.so/Union-Find-d50eb1b078994ce3809a2f311c309f7b?pvs=21) 

```python
class Solution(object):
    def countComponents(self, n, edges):
        self.parent = [x for x in range(n)]
        self.size = [1] * n
        self.count = n # init

        for A, B in edges:
            self.union(A, B)
        
        return self.count
    
    def find(self, node):
        # path compression
        while node != self.parent[node]:
            self.parent[node] = self.parent[self.parent[node]]
            node = self.parent[node]
        return node
    
    def union(self, A, B):
        root1 = self.find(A)
        root2 = self.find(B)
        
        if root1 == root2:
            return
        
        # merge smaller set into larger set
        if self.size[root1] > self.size[root2]:
            self.parent[root2] = root1
            self.size[root1] += 1
        else:
            self.parent[root1] = root2
            self.size[root2] += 1
        
        self.count -= 1

# Time: O(V + E)
    # O(V) time to initialize "parent" array.
    # Iterate through all E edges, perform ~O(1) work for each edge.

# Space: O(V)
    # "parent" array and "size" array, one entry for every node.
```

### DFS

Start from a node, visit everything until finish its DFS.

```python
class Solution(object):
    def countComponents(self, n, edges):
        self.adj = defaultdict(list)
        self.visited = set()
        count = 0

        for A, B in edges:
            self.adj[A].append(B)
            self.adj[B].append(A)
        
        for i in range(n):
            if i not in self.visited:
                count += 1
                self.dfs(i)
        
        return count

    def dfs(self, node):
        if node in self.visited:
            return
        
        self.visited.add(node)
        for nei in self.adj[node]:
            self.dfs(nei)

# Time: O(E + V)
    # O(E) to build adjacency list
    # During DFS traversal we visit each vertex exactly once.
# Space: O(E + V)
    # O(E) for adjacency list.
    # O(V) for visited set.
```