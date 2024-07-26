# 23. Merge k Sorted Lists

Date (of first attempt): 03/30/2024  
Difficulty: Hard  
Topics: Divide & Conquer, Linked List, Priority Queue  
Notes: Use dummy head (return head.next).
Using heapq w/ priority:
- hq.heappush(minPQ, (node.val, node))
- front = hq.heappop(minPQ)[1] # don’t get tuple 

Question Link: https://leetcode.com/problems/merge-k-sorted-lists/description/


## Problem

```
You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

 

Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
Example 2:

Input: lists = []
Output: []
Example 3:

Input: lists = [[]]
Output: []
```

```
push in heads with prioirty value it's value (minPQ)
```

## Solutions

### Priority Queue

```python
import heapq as hq
class Solution(object):
    def mergeKLists(self, lists):
        if not lists: return None

        minPQ = []
        for root in lists:
            if root:
                hq.heappush(minPQ, (root.val, root))
        
        if not minPQ: return None

        head = ListNode()
        res = head

        while minPQ:
            front = hq.heappop(minPQ)[1] # smallest value
            if front.next:
                hq.heappush(minPQ, (front.next.val, front.next)) 
            head.next = front
            head = head.next
        
        return res.next

# Time: O(Nlogk) where N is # of nodes. Push each node (logk) into PQ.
# Space: O(k); PQ maintains max size of k nodes (# of ptrs)
```

### Divide & Conquer

When pairing lists together, pair list at “i” with list at “i + gap”. Increase this gap until you reach total number of lists.

```python
import heapq as hq
class Solution(object):
    def mergeKLists(self, lists):
        if not lists: return None
        total = len(lists)
        gap = 1
        while gap < total: 
            for i in range(0, total-gap, gap*2):
                lists[i] = self.merge2Lists(lists[i], lists[i+gap])
            gap *= 2
        return lists[0]
    
    def merge2Lists(self, l1, l2): # O(N)
        if not l1: return l2
        if not l2: return l1

        res = head = ListNode(0)
        while l1 and l2:
            if l1.val <= l2.val:
                head.next = l1
                l1 = l1.next
            else:
                head.next = l2
                l2 = l2.next
            head = head.next
        
        head.next = l1 if l1 is not None else l2
        return res.next

# logk levels
# ea. merge op w/ 2 LLs = O(n) where n is avg length of given LLs
# ea. level has SUM(ea. merge op) = O(N) where N is total # nodes
# logk levels, O(N) work at ea. level -> O(Nlogk)

# space is O(1) b/c no PQ needed
```