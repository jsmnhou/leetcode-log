# 49. Group Anagrams

Date (of first attempt): 04/04/2024  
Difficulty: Medium  
Topics: Arrays  
Quick Notes: Uses: defaultdict, sorted(s), list comprehension  
Question Link: https://leetcode.com/problems/group-anagrams/description/  

## Problem

```
Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
Example 2:

Input: strs = [""]
Output: [[""]]
Example 3:

Input: strs = ["a"]
Output: [["a"]]
```

## Solutions

### Sorted string as key

```python
class Solution(object):
    def groupAnagrams(self, strs):
        mapping = defaultdict(list)
        for s in strs:
            mapping[tuple(sorted(s))].append(s)
        return mapping.values()
        
# Time: O(NKlogK)
	# N is length of strs, K is max length of an s in strs
	# Sorting takes O(KlogK), do this N times.
# Space: O(NK)
	# Every s in strs is "array of chars" (has max length K) 
	# We return lists of size N, where ea. elt of those lists are arrays of chars.
```

### Counts of chars as key

Less space when have words that are obnoxiously long. Such as: “aaaaaaaaaabb” → “a10b2.”

```python
class Solution(object):
    def groupAnagrams(self, strs):
        mapping = defaultdict(list)

        for s in strs:
            d = Counter([c for c in s])
            sorted_d = dict(sorted(d.items()))
            key = ""
            for c, i in sorted_d.items():
                key += c + str(i) # a:1 -> 'a1'
            mapping[key].append(s)
        
        return mapping.values()

# Time: O(NK)
	# N is length of strs, K is max length of an s in strs
	# Counting "c for c in s" takes K time.
# Space: O(NK)
	# Every s in strs is "array of chars" (has max length K) 
	# We return lists of size N, where ea. elt of those lists are arrays of chars.
	
	
    # ["eat","tea","tan","ate","nat","bat"]

    # for each string, count how many occurences for ea. char
    # eat -> a:1, e:1, t:1
    # tea -> a:1, e:1, t:1

    # encode this into a key string
        # "a1e1t1"
    # map["a1e1t1"] = [ "eat", "tea" ] # list of strings
```