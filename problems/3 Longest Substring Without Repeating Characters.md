# 3. Longest Substring Without Repeating Characters

Date (of first attempt): 03/24/2024  
Difficulty: Medium  
Topics: Sliding Window  
Question Link: https://leetcode.com/problems/longest-substring-without-repeating-characters/description/

## Problem

```
Given a string s, find the length of the longest 
substring without repeating characters.


Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solutions

### Sliding Window

Move “left” ptr up until we no longer have the repeat char. New length is (right - left + 1).

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        chars = Counter()
        left = right = 0

        longest = 0
        while right < len(s):
            r = s[right]
            chars[r] += 1

            while chars[r] > 1:
                l = s[left]
                chars[l] -= 1
                left += 1

            longest = max(longest, right - left + 1)
            right += 1

        return longest
    
    # Time: O(2n) = O(n). Worst case each char visited twice by i and j.
    # Space: O(min(n, m)) where n = len(s) and m = len(alphabet). Size of sliding window
```

### Sliding Window Optimized

When we found a repeat, we can move “start” immediately to 1 + last seen location (instead of ++left slowly).

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        last_seen = dict()
        l = length = 0

        for r in range(len(s)):
            c = s[r]
            if c in last_seen:
                if last_seen[c] >= l:
                    l = last_seen[c] + 1

            length = max(length, r-l+1)
            last_seen[c] = r
        
        return length

# Time: O(n). i iterates n times always.
# Space: O(min(n, m))
```