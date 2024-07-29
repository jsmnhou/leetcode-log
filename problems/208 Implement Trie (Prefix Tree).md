# 208. Implement Trie (Prefix Tree)

Date (of first attempt): 04/01/2024  
Difficulty: Medium  
Topics: Trie  
Question Link: https://leetcode.com/problems/implement-trie-prefix-tree/description/  

## Problem

```
A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

Trie() Initializes the trie object.
void insert(String word) Inserts the string word into the trie.
boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

Example 1:

Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

## Solutions

### Standard Implementation

```python
class Trie(object):

    def __init__(self):
        self.root = {}
        
    def insert(self, word):
        curr = self.root
        for c in word:
            if c not in curr:
                curr[c] = {}
            curr = curr[c]
        curr["*"] = "*"

    def search(self, word):
        curr = self.root
        for c in word:
            if c not in curr:
                return False
            curr = curr[c]
        return "*" in curr

    def startsWith(self, prefix):
        curr = self.root
        for c in prefix:
            if c not in curr:
                return False
            curr = curr[c]
        return True
```