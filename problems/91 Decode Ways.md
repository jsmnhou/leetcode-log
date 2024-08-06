# 91. Decode Ways

Date (of first attempt): 04/04/2024
Difficulty: Medium
Topics: 1-D Dynamic Progamming
Quick Notes: Indexing is weird. F(i) = # ways to decode up to index i-1 in string. So think of dp[2] as “2nd char” in string, ex: “1234” → ‘2’.
Question Link: https://leetcode.com/problems/decode-ways/description/
1D: Yes
2D: Yes
7D: Yes
15D: No
1M: No
3M: No
Days since first attempt: -124
Last edited time: April 11, 2024 12:52 PM

## Problem

```
A message containing letters from A-Z can be encoded into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

The test cases are generated so that the answer fits in a 32-bit integer.

 

Example 1:

Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
Example 2:

Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
Example 3:

Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
```

```

```

## Solutions

### Bottom-up

`F(i) = # ways to decode up to i-1 in s.` Or can say as, F(1) means how many mappings for 1 character in s (first char).

Note: `F(2)` means “2nd char” in s. If `s=”1234”` ,`F(2) = ‘2’` .

Init `dp[0] = 1`, conceptually “# of ways to decode up the no chars in s.”

`dp[i]` can get the baton from either `i-1` or `i-2` (or both). If “ith char” (aka “char at index `i-1` in s”) is valid single, add `dp[i-1]`. If “ith char” combined with the previous is valid double, add `dp[i-2]`. 

> Think of it as:

If I can form a valid single (w/ myself), if there are M ways to reach dp[i-1], there are now M ways to reach me since I can just tack myself behind each way.

If I can form a valid double (w/prev and myself), if there N ways to reach dp[i-2], there are now N (more) ways to reach me since I can just tack (prev+myself) behind each way.
> 

```python
class Solution(object):
    def numDecodings(self, s):
        if not s: return 0

        n = len(s)
        dp = [0] * (n+1)
        dp[0] = 1
        dp[1] = 0 if s[0] == '0' else 1

        for i in range(2, n+1):
            if s[i-1] != '0':
                dp[i] += dp[i-1]
            if 10 <= int(s[i-2] + s[i-1]) <= 26:
                dp[i] += dp[i-2]
        
        return dp[n]

        # Time: O(N) where N is length of string.
        # Space: O(N) for dp array.

        # 126
        # dp[0] = 1
        # dp[1] = 1
        # dp[2] = 1+1
        # dp[3] = 2+1
    
        # F(0) = 1, F(1) = 0 or 1(if not '0')

        # F(i) = # ways to decode up to index i-1 in s
        # output F(n) at the end

        # F(i) = F(i-1) + F(i-2)
            # if s[i-1] is valid (not 0):
                # include F(i-1)
            # if s[i-2]+s[i-1] form valid (b/w 10 to 26):
                # include F(i-2)
```