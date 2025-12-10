---
tags:
  - difficulty/medium
  - technique/frequency-count
  - technique/grouping
  - pattern/anagram
  - company/google
  - company/amazon
  - status/solved
---

# Group Anagrams

**Source:** LeetCode #49  
**Difficulty:** Medium  
**Date Solved:** 2024-12-06

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An anagram is a word formed by rearranging the letters of another word.

## Approach

**Primary Technique:** [[techniques/grouping-by-key]]

Group strings by a canonical key that's the same for all anagrams. Two common key strategies:
1. **Sorted string:** Sort characters and use as key
2. **Character count tuple:** Use frequency tuple as key

## Solution 1: Sorted String Key (Preferred)

```python
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)
        for s in strs:
            key = ''.join(sorted(s))
            groups[key].append(s)
        return list(groups.values())
```

**Complexity:**
- Time: O(n * k log k) - n strings, k is max string length
- Space: O(n * k) - storing all strings

## Solution 2: Counter Tuple Key

```python
from collections import Counter, defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        hit = defaultdict(list)
        for s in strs:
            key = tuple(sorted(Counter(s).items()))
            hit[key].append(s)
        return list(hit.values())
```

**Complexity:**
- Time: O(n * k) - counting is linear
- Space: O(n * k)

## Trade-off Analysis

| Approach | Time | When to Use |
|----------|------|-------------|
| Sorted string | O(n * k log k) | Shorter strings, simpler code |
| Counter tuple | O(n * k) | Very long strings where sorting is expensive |

For typical interview constraints, sorted string is cleaner and preferred.

## Key Insights

- `defaultdict(list)` eliminates key existence checks
- Anagrams share the same sorted form or character frequency
- Tuple is hashable (can be dict key), list is not
- `sorted()` returns a list, need `''.join()` to make string key

## Related Problems

- [[solutions/arrays/valid-anagram]] - Simpler: compare two strings
- [[solutions/arrays/two-sum]] - Also uses hashmap grouping

## Techniques Used

- [[techniques/grouping-by-key]]
- [[techniques/frequency-count]]
- [[methods/collections/defaultdict]]
- [[methods/collections/counter]]
- [[methods/built-ins/sorted]]
