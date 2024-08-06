# 143. Reorder List

Date (of first attempt): 04/01/2024
Difficulty: Medium
Topics: Linked List
Quick Notes: Slow/fast ptrs, Reverse LLs, Merge 2 LLs
Question Link: https://leetcode.com/problems/reorder-list/description/
1D: Yes
2D: Yes
7D: Yes
15D: Yes
1M: No
3M: No
Days since first attempt: -126
Last edited time: April 17, 2024 12:49 PM
Other tags: !!!

## Problem

```
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

Example:
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

```

```

![Untitled](143%20Reorder%20List%20ee1cd01022a34b4cb28b40046beea9f9/Untitled.png)

## Solutions

### Using pointers

Major concepts:

- Slow/fast ptr to find middle node
- How to reverse a LL
- How to combine 2 LLs

```python
class Solution(object):
    def reorderList(self, head):
        if not head: return

        # slow/fast ptrs
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        middle = slow

        # reverse LL
        prev, curr = None, middle
        while curr:
            tmp = curr.next
            curr.next = prev
            prev = curr
            curr = tmp
            
        # at this point, looks like:
        # [1, 2, 3, 4]
        # head: 1 -> 2 -> 3 -> None
        # prev: 4 -> 3 -> None       

        # combine            
        p1, p2 = head, prev
				while p2.next:
					tmp1 = p1.next
					p1.next = p2
					p1 = tmp1
					
					tmp2 = p2.next
					p2.next = tmp1 # or p1
					p2 = tmp2
            
   # Time: O(N)
	      # slow/fast takes O(N)
	      # reverse takes O(N/2)
	      # combine takes O(N/2)
	 # Space: O(1)
```

### Using array

Works, but less space efficient. Also doesn’t demonstrate ability to manipulate pointers in LLs.

```python
class Solution(object):
    def reorderList(self, head):
        if not head: return None

        res = ListNode() # dummy
        array = []

        curr = head
        while curr:
            tmp = curr.next
            curr.next = None
            array.append(curr)
            curr = tmp

        m = int(ceil(len(array) / 2.0))
        front = array[:m]
        back = array[m:]

        p1, p2 = 0, len(back)-1
        while p1 < len(front) and p2 >= 0:
            res.next = front[p1]
            res.next.next = back[p2]
            p1 += 1
            p2 -= 1
            res = res.next.next
        
        if p1 < len(front): # odd length
            res.next = front[len(front)-1]
        
        return res.next
    
    # Time: O(N)
        # O(N) for first pass through entire list
        # O(N) for constructing 
    # Space: O(N)
        # need array to hold all nodes
```