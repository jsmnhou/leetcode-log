# 79. Word Search

Date (of first attempt): 04/09/2024  
Difficulty: Medium  
Topics: Backtracking, DFS  
Quick Notes: Backtracking template. Use DFS for backtracking, don’t try to fw BFS.  
Question Link: https://leetcode.com/problems/word-search/description/  

## Problem

```
Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

 

Example 1:
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

![Untitled](79%20Word%20Search%205a65441e01274aae8a9e324011c7f8a6/Untitled.png)

## Solutions

### Backtracking

Initially tried this problem with iterative BFS and maintaining visited set. However this is difficult to implement: must somehow be able to “unmark” a cell if current route doesn’t work.

This ties into how backtracking problems are generally structured:

```python
def backtrack(candidate):
    if find_solution(candidate):
        output(candidate)
        return
    
    # iterate all possible candidates.
    for next_candidate in list_of_candidates:
        if is_valid(next_candidate):
            # try this partial candidate solution
            place(next_candidate)
            # given the candidate, explore further.
            backtrack(next_candidate)
            # backtrack
            remove(next_candidate)
```

With BFS, it’s hard to do the “remove(next_candidate)” part. So, just stick to DFS after identifying that it is a backtracking problem.

Also, can use extra checks before starting backtracking to avoid wasting time on “bad” boards (see “searching pruning”). 

```python
class Solution(object):
    def exist(self, board, word):
        self.ROWS = len(board)
        self.COLS = len(board[0])
        self.board = board

        # search pruning:
        total_chars = self.ROWS * self.COLS
        freq = defaultdict(int)

        for r in range(self.ROWS):
            for c in range(self.COLS):
                freq[self.board[r][c]] += 1

        # check length of word <= total chars in board
        if total_chars < len(word):
            return False

        # check board has enough freq for ea. char in word
        counter = Counter(word)
        for char, count in counter.items():
            if freq[char] < count:
                return False

        # after checks pass, start backtrack
        for r in range(self.ROWS):
            for c in range(self.COLS):
                if self.backtrack(r, c, word):
                    return True
        return False

    
    def backtrack(self, r, c, suffix):
        if len(suffix) == 0:
            return True
        
        if r < 0 or c < 0 or r >= self.ROWS or c >= self.COLS \
            or self.board[r][c] != suffix[0]:
            return False
        
        # try this partial candidate solution (mark choice to continue exploring)
        self.board[r][c] = '#'

        ret = False
        dirs = [(1, 0), (0, 1), (-1, 0), (0, -1)]

        for x, y in dirs:
            ret = self.backtrack(r + x, c + y, suffix[1:])
            if ret: break
        
        # backtrack (revert change, a clean slate and no side-effect)
        self.board[r][c] = suffix[0]
        return ret

# Time: O(N * 3^L) where N is number of cells in board and L is length of word.
    # Initially, have at most 4 dirs to explore, but choices are reduced to 3 b/c we won't go back to where we come from.
    # Can view execution trace as 3-ary (ternary) tree; # of nodes traversed = 3^L. 
    # Worst case, we iterate through all N cells for backtracking.

# Space: O(L)
    # Recursion call stack is max the length of the word.
```