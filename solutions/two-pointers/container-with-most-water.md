# Container With Most Water

**Source:** LeetCode #11  
**Difficulty:** Medium  
**Date Solved:** 2026-01-03

## Problem Statement

Given `n` non-negative integers representing the heights of vertical lines, find two lines that together with the x-axis form a container that holds the most water.

## Approach

**Primary Technique:** [[techniques/two-pointers]]

The key insight is that the area is limited by the **shorter** line. Starting with the widest container (left=0, right=n-1), we can only potentially increase the area by moving the pointer at the shorter line inward.

**Why move the shorter pointer?**
- Moving the taller one can only decrease or maintain area (width decreases, height can't increase beyond the shorter line)
- Moving the shorter one might find a taller line that increases the limiting height

## Solution

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        max_water = 0

        while left < right:
            width = right - left
            h = min(height[left], height[right])
            max_water = max(max_water, width * h)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_water
```

## Optimized Solution (Skip Duplicates)

This version skips positions that can't possibly improve the result:

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        max_water = 0

        while left < right:
            h_left, h_right = height[left], height[right]
            width = right - left
            
            if h_left < h_right:
                max_water = max(max_water, width * h_left)
                # Skip all heights <= current left height
                while left < right and height[left] <= h_left:
                    left += 1
            else:
                max_water = max(max_water, width * h_right)
                # Skip all heights <= current right height
                while left < right and height[right] <= h_right:
                    right -= 1

        return max_water
```

**Why this is faster in practice:**
- Skips positions with equal or shorter heights (they can't improve max area)
- Same O(n) worst case, but fewer iterations on average
- Particularly effective when there are many duplicate or small values

## Complexity Analysis

- **Time Complexity:** O(n) - each pointer moves at most n times total
- **Space Complexity:** O(1) - only using pointers and variables

## Key Insights

- **Greedy choice is safe**: Moving the shorter pointer never misses the optimal
- **Width vs Height trade-off**: Starting wide guarantees we explore all possibilities
- **Equal heights**: Moving either pointer is fine (often move right by convention)
- **Skip optimization**: When moving, skip past heights that can't improve

## Visual Intuition

```
heights: [1, 8, 6, 2, 5, 4, 8, 3, 7]
          L                       R
          
Area = min(1, 7) * 8 = 8
Move L (shorter) →

heights: [1, 8, 6, 2, 5, 4, 8, 3, 7]
             L                    R
             
Area = min(8, 7) * 7 = 49  ← Max found!
```

## Related Problems

- [[solutions/two-pointers/trapping-rain-water]] - Similar concept, sum all trapped water
- [[solutions/two-pointers/3sum]] - Two-pointer after fixing one element

## Techniques Used

- [[techniques/two-pointers]]
