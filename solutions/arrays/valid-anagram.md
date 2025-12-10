---
tags:
  - difficulty/easy
  - technique/frequency-count
  - pattern/anagram
  - company/google
  - company/amazon
  - status/solved
---

# Valid Anagram

**Source:** LeetCode #242  
**Difficulty:** Easy  
**Date Solved:** 2024-12-06

## Problem Statement

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

An anagram is a word formed by rearranging the letters of another word using all the original letters exactly once.

## Approach

**Primary Technique:** [[techniques/frequency-count]]

Use Counter to count character frequencies in both strings, then compare. If the frequency maps are equal, strings are anagrams.

**Why it works:**
- Anagrams have identical character frequencies
- Counter handles the counting automatically
- Direct equality comparison of Counters

## Solution

```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

## Complexity Analysis

- **Time Complexity:** O(n) - counting characters in both strings
- **Space Complexity:** O(k) - where k is the size of character set (26 for lowercase English)

## Key Insights

- One-liner solution using Counter comparison
- Counter handles edge cases (empty strings, different lengths)
- Alternative: sort both strings and compare - O(n log n) time
- For follow-up with Unicode: Counter still works, but space becomes O(n)

## Alternative Solutions

### Sorting Approach
```python
def isAnagram(self, s: str, t: str) -> bool:
    return sorted(s) == sorted(t)
```
- Time: O(n log n), Space: O(n)
- Simpler but slower

### Manual Counting
```python
def isAnagram(self, s: str, t: str) -> bool:
    if len(s) != len(t):
        return False
    count = {}
    for c in s:
        count[c] = count.get(c, 0) + 1
    for c in t:
        count[c] = count.get(c, 0) - 1
        if count[c] < 0:
            return False
    return True
```
- Time: O(n), Space: O(k)
- More explicit, single dict

## Related Problems

- [[solutions/arrays/contains-duplicate]] - Also about detecting duplicates/matches
- [[solutions/arrays/group-anagrams]] - Extension: group strings by anagram

## Techniques Used

- [[techniques/frequency-count]]
- [[methods/collections/counter]]
