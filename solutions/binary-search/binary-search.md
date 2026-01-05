# Binary Search

**Source:** LeetCode #704  
**Difficulty:** Easy  
**Date Solved:** 2026-01-03

## Problem Statement

Given a sorted array of integers `nums` and a target value, return the index of target if found, otherwise return -1.

## Approach

**Primary Technique:** [[notes/techniques/binary-search]]

Classic binary search on a sorted array:
1. Maintain `left` and `right` pointers defining the search range
2. Check middle element: if target, return; if less, search right; if greater, search left
3. When `left > right`, target doesn't exist

## Solution

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left <= right:
            mid = (left + right) // 2
            
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return -1
```

## Complexity Analysis

- **Time Complexity:** O(log n) - halving search space each iteration
- **Space Complexity:** O(1) - only using pointers

## Key Insights

1. **Condition `left <= right`:** Includes single-element check
2. **`mid + 1` and `mid - 1`:** Avoids infinite loop by excluding mid
3. **Integer overflow:** In other languages, use `left + (right - left) // 2`
4. **Sorted array required:** Binary search only works on sorted data

## Template Variations

### Find Leftmost (Lower Bound)
```python
def lower_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

### Find Rightmost (Upper Bound)
```python
def upper_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left
```

## Related Problems

- [[solutions/binary-search/search-2d-matrix]] - Binary search on flattened matrix
- [[solutions/binary-search/search-rotated-sorted-array]] - Modified for rotation
- [[solutions/binary-search/find-minimum-rotated-sorted-array]] - Find pivot

## Techniques Used

- [[notes/techniques/binary-search]]
