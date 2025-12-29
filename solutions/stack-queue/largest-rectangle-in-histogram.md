---
tags:
  - difficulty/hard
  - technique/monotonic-stack
  - pattern/span-calculation
  - source/leetcode
  - status/solved
---

# Largest Rectangle in Histogram

**Source:** LeetCode #84  
**Difficulty:** Hard  
**Date Solved:** 2024-12-28

## Problem Statement

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

## Approach 1: Brute Force (TLE)

For each bar, expand left and right to find how far the rectangle can extend.

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        max_area = 0

        for i in range(n):
            # expand to the left
            left = i
            while left >= 0 and heights[left] >= heights[i]:
                left -= 1

            # expand to the right
            right = i
            while right < n and heights[right] >= heights[i]:
                right += 1

            # compute width and area
            width = right - left - 1
            area = heights[i] * width
            max_area = max(max_area, area)

        return max_area
```

**Time:** O(n²) — for each bar, we potentially scan all bars  
**Space:** O(1)

**Why TLE?** For arrays like `[1, 2, 3, 4, 5, ..., n]`, each bar scans almost all previous bars.

---

## Approach 2: Monotonic Stack (Optimal)

**Primary Technique:** [[notes/techniques/monotonic-stack]]

### Key Insight

For each bar, we need to find:
- **Left boundary:** First bar to the left that is **shorter** (Previous Smaller Element)
- **Right boundary:** First bar to the right that is **shorter** (Next Smaller Element)

A **monotonic increasing stack** can find both boundaries in O(n) total!

### How It Works

1. Maintain a stack of indices where heights are in **increasing order**
2. When we see a bar **shorter** than stack top:
   - Pop the top — this bar just found its right boundary!
   - Its left boundary is the new stack top (or -1 if empty)
   - Calculate area: `height[popped] × (right - left - 1)`
3. After processing all bars, pop remaining elements (their right boundary is `n`)

### Solution

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []  # stores indices
        max_area = 0
        n = len(heights)
        
        for i in range(n + 1):
            # Use 0 as sentinel for final cleanup
            curr_height = heights[i] if i < n else 0
            
            while stack and curr_height < heights[stack[-1]]:
                h = heights[stack.pop()]
                # Left boundary: stack top (or -1 if empty)
                # Right boundary: current index i
                width = i if not stack else i - stack[-1] - 1
                max_area = max(max_area, h * width)
            
            stack.append(i)
        
        return max_area
```

**Time:** O(n) — each element pushed and popped at most once  
**Space:** O(n) — for the stack

### Alternative: Explicit Left/Right Arrays

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        left = [0] * n   # index of previous smaller element
        right = [0] * n  # index of next smaller element
        stack = []
        
        # Find previous smaller element for each index
        for i in range(n):
            while stack and heights[stack[-1]] >= heights[i]:
                stack.pop()
            left[i] = stack[-1] if stack else -1
            stack.append(i)
        
        stack.clear()
        
        # Find next smaller element for each index
        for i in range(n - 1, -1, -1):
            while stack and heights[stack[-1]] >= heights[i]:
                stack.pop()
            right[i] = stack[-1] if stack else n
            stack.append(i)
        
        # Calculate max area
        max_area = 0
        for i in range(n):
            width = right[i] - left[i] - 1
            max_area = max(max_area, heights[i] * width)
        
        return max_area
```

---

## Complexity Analysis

| Approach | Time | Space |
|----------|------|-------|
| Brute Force | O(n²) | O(1) |
| Monotonic Stack | O(n) | O(n) |

---

## Visual Walkthrough

```
heights = [2, 1, 5, 6, 2, 3]
indices =  0  1  2  3  4  5

Stack processing (storing indices):

i=0: push 0              stack=[0]
i=1: heights[1]=1 < heights[0]=2
     → pop 0: h=2, width=1, area=2
     push 1              stack=[1]
i=2: push 2              stack=[1,2]
i=3: push 3              stack=[1,2,3]
i=4: heights[4]=2 < heights[3]=6
     → pop 3: h=6, width=4-2-1=1, area=6
     heights[4]=2 < heights[2]=5
     → pop 2: h=5, width=4-1-1=2, area=10 ⭐
     push 4              stack=[1,4]
i=5: push 5              stack=[1,4,5]
i=6: cleanup with height=0
     → pop 5: h=3, width=6-4-1=1, area=3
     → pop 4: h=2, width=6-1-1=4, area=8
     → pop 1: h=1, width=6, area=6

Max area = 10
```

---

## Key Insights

1. **Stack stores indices, not heights** — we need positions for width calculation
2. **Increasing stack = find smaller elements** — when order breaks, we found a boundary
3. **Sentinel trick**: Adding a 0-height bar at the end forces all remaining bars to be processed
4. **Width formula**: `right - left - 1` where left and right are the boundaries (exclusive)

## Common Mistakes

- Forgetting to process remaining elements in the stack
- Off-by-one errors in width calculation
- Using decreasing stack instead of increasing (wrong direction!)

## Related Problems

- [[solutions/stack-queue/daily-temperatures]] - Next greater element pattern
- [[solutions/stack-queue/trapping-rain-water]] - Similar span calculation
- [[solutions/stack-queue/maximal-rectangle]] - 2D extension of this problem

## Techniques Used

- [[notes/techniques/monotonic-stack]] - Core technique for O(n) solution
- [[notes/patterns/span-calculation]] - Finding left/right extents

## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-12-28 | 3/5 | First attempt was brute force TLE, understood monotonic stack |

## Bug Pattern

| Bug Type | What Happened |
|----------|---------------|
| TLE from O(n²) | Used nested loops instead of monotonic stack |
