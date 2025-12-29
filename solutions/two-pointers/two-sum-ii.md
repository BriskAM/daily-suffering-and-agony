---
tags:
  - difficulty/medium
  - technique/two-pointers
  - pattern/pair-sum
  - source/leetcode
  - status/solved
---

# Two Sum II - Input Array Is Sorted

**Source:** LeetCode #167  
**Difficulty:** Medium  
**Date Solved:** 2024-12-28

## Problem Statement

Given a **1-indexed** array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number.

Return the indices of the two numbers (1-indexed) as an integer array `[index1, index2]`.

You may not use the same element twice. The solution is guaranteed to exist and be unique.

## Approach

**Primary Technique:** [[notes/techniques/two-pointers]]

### Key Insight

Since the array is **sorted**, we can use two pointers from opposite ends:
- If `sum < target` → move left pointer right (need larger value)
- If `sum > target` → move right pointer left (need smaller value)
- If `sum == target` → found it!

This exploits the sorted property to eliminate half the search space with each comparison.

## Solution

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        
        while left < right:
            s = numbers[left] + numbers[right]
            
            if s == target:
                return [left + 1, right + 1]
            elif s < target:
                left += 1
            else:
                right -= 1
```

## Complexity Analysis

- **Time Complexity:** O(n) — each pointer moves at most n times
- **Space Complexity:** O(1) — only two pointers

## Why Two Pointers Works Here

```
numbers = [2, 7, 11, 15], target = 9

left=0, right=3: 2 + 15 = 17 > 9  → right--
left=0, right=2: 2 + 11 = 13 > 9  → right--
left=0, right=1: 2 + 7  = 9  ✓   → return [1, 2]
```

The sorted property guarantees:
- Moving left pointer right **increases** the sum
- Moving right pointer left **decreases** the sum

So we can deterministically narrow down to the answer!

## Comparison with Two Sum I

| Problem | Array | Technique | Time | Space |
|---------|-------|-----------|------|-------|
| Two Sum I (#1) | Unsorted | HashMap | O(n) | O(n) |
| **Two Sum II (#167)** | **Sorted** | **Two Pointers** | O(n) | **O(1)** |

The sorted constraint allows us to avoid the hashmap entirely!

## Key Insights

1. **1-indexed return**: Problem asks for 1-based indices (`left + 1`, `right + 1`)
2. **Guaranteed solution**: No need to handle "not found" case
3. **No duplicates needed**: Each element used at most once
4. **Sorted is the key**: Without sorting, this approach doesn't work

## Edge Cases

- Two elements: `[1, 2]`, target = 3 → `[1, 2]`
- Same values: `[3, 3]`, target = 6 → `[1, 2]`
- Negative numbers: `[-1, 0]`, target = -1 → `[1, 2]`

## Related Problems

- [[solutions/arrays/two-sum]] - Unsorted version (use hashmap)
- [[solutions/two-pointers/3sum]] - Extension with three pointers
- [[solutions/two-pointers/container-with-most-water]] - Another two pointer pattern

## Techniques Used

- [[notes/techniques/two-pointers]] - Opposite direction pattern

## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-12-28 | 5/5 | Classic two pointer pattern, well understood |
