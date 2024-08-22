# 102. Binary Tree Level Order Traversal

Date (of first attempt): 08/06/2024  
Difficulty: Medium  
Topics: BFS, Tree  
Quick Notes: To record layers/levels, use current length of queue = # nodes in current layer.  
Question Link: https://leetcode.com/problems/binary-tree-level-order-traversal/description/  

## Problem

```
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

 

Example 1:

Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
Example 2:

Input: root = [1]
Output: [[1]]
Example 3:

Input: root = []
Output: []
```

![https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)


## Solutions

### BFS

Move “left” ptr up until we no longer have the repeat char. New length is (right - left + 1).

```python
class Solution(object):
    def levelOrder(self, root):
        if not root:
            return []
        
        q = [root]
        res = []

        while q:
            layer_count = len(q) # num nodes in this layer
            layer_array = []

            for i in range(layer_count):
                curr = q.pop(0)
                layer_array.append(curr.val)
                if curr.left:
                    q.append(curr.left)
                if curr.right:
                    q.append(curr.right)
            
            res.append(layer_array)
            layer_array = []

        return res

# for recording by layer:
    # length of queue (N) = # nodes for this layer
        # (b/c queued all children last iteration)
    # once N nodes traversed,
        # start new layer
        
# Time: O(n) iterate through ea node once
# Space: O(n) q has max size of total num of nodes
```