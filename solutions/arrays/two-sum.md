---
tags:
  - difficulty/easy
  - technique/hashmap
  - pattern/complement
  - company/google
  - company/amazon
  - company/facebook
  - status/solved
---

# Two Sum

**Source:** LeetCode #1  
**Difficulty:** Easy  
**Date Solved:** 2024-12-06

## Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Approach

**Primary Technique:** [[techniques/hashmap-complement]]

Use a hashmap to store seen numbers and their indices. For each number, calculate its complement (`target - num`). If the complement exists in the hashmap, we found our pair.

**Why it works:**
- Instead of checking all pairs (O(n^2)), we do a single pass
- The hashmap gives O(1) lookup for complements
- We build the hashmap as we go, so we only find valid pairs

## Solution

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hit = {}
        for i, n in enumerate(nums):
            m = target - n
            if m in hit.keys():
                return [hit[m], i]
            hit.update({n: i})
```

## Complexity Analysis

- **Time Complexity:** O(n) - single pass through the array
- **Space Complexity:** O(n) - hashmap stores up to n elements

## Key Insights

- The complement pattern: instead of looking for `a + b = target`, look for `target - a` in seen values
- Building the hashmap while iterating avoids using the same element twice
- Using `enumerate()` gives both index and value in one loop

## Related Problems

- [[solutions/arrays/three-sum]] - Extension to finding triplets
- [[solutions/arrays/two-sum-ii]] - Sorted array variant (use two pointers)
- [[solutions/arrays/four-sum]] - Extension to finding quadruplets

## Techniques Used

- [[techniques/hashmap-complement]]
- [[methods/built-ins/enumerate]]
- [[methods/built-ins/dict]]
