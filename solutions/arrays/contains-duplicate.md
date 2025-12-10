---
tags:
  - difficulty/easy
  - technique/hashset
  - pattern/duplicate-detection
  - company/google
  - company/amazon
  - status/solved
---

# Contains Duplicate

**Source:** LeetCode #217  
**Difficulty:** Easy  
**Date Solved:** 2024-12-06

## Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

## Approach

**Primary Technique:** [[techniques/hashset-lookup]]

Use a HashSet to track seen elements. For each number, check if it exists in the set. If yes, we found a duplicate. If no, add it to the set and continue.

**Why it works:**
- Set provides O(1) lookup and insertion
- We can detect duplicates in a single pass
- Space trade-off for time efficiency

## Solution

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for n in nums:
            if n in seen:
                return True
            seen.add(n)
        return False
```

## Complexity Analysis

- **Time Complexity:** O(n) - single pass through array
- **Space Complexity:** O(n) - set stores up to n elements

## Key Insights

- Early return optimization: stop as soon as duplicate found
- Alternative: `return len(nums) != len(set(nums))` - more concise but always processes entire array
- Set is the go-to for duplicate detection problems

## Related Problems

- [[solutions/arrays/two-sum]] - Also uses hashing for lookups
- [[solutions/arrays/valid-anagram]] - Uses Counter for frequency comparison

## Techniques Used

- [[techniques/hashset-lookup]]
- [[methods/built-ins/set]]
