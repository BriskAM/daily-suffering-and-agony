# Trapping Rain Water

**Source:** LeetCode #42  
**Difficulty:** Hard  
**Date Solved:** 2026-01-03

## Problem Statement

Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Approach

**Primary Technique:** [[techniques/two-pointers]]  
**Secondary Techniques:** [[notes/techniques/greedy-comparison]]

### Key Insight

Water at any position = `min(left_max, right_max) - height[i]`

Instead of precomputing left_max and right_max arrays (O(n) space), we use two pointers:
- If `height[left] < height[right]`, we know `left_max` is the limiting factor
- If `height[right] <= height[left]`, we know `right_max` is the limiting factor

**Why this works:**
- When `height[left] < height[right]`, there exists at least one bar on the right ≥ `height[right]`
- So `right_max >= height[right] > height[left]`
- Therefore water level is bounded by `left_max`, not `right_max`

## Solution

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        left_max = right_max = 0
        water = 0

        while left < right:
            if height[left] < height[right]:
                if height[left] >= left_max:
                    left_max = height[left]
                else:
                    water += left_max - height[left]
                left += 1
            else:
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    water += right_max - height[right]
                right -= 1

        return water
```

## Complexity Analysis

- **Time Complexity:** O(n) - single pass with two pointers
- **Space Complexity:** O(1) - only tracking max values and pointers

## Key Insights

1. **Two-pointer invariant:** The side with smaller height is "safe" to process
2. **No need for precomputation:** We track running max as we go
3. **Water trapped at position:** `max_so_far - current_height` (if positive)
4. **Update max vs add water:** If current ≥ max, update max (no water); else add water

## Visual Walkthrough

```
height = [0,1,0,2,1,0,1,3,2,1,2,1]

        #
    #   ##
 #  ## ####
_#_##_####_#

      L                 R    left_max  right_max  water
      0                11    0         0          0
height[0]=0 < height[11]=1
  left_max = 0, move L→
      
      1                11    0         0          0
height[1]=1 >= left_max
  left_max = 1, move L→

      2                11    1         0          0
height[2]=0 < left_max=1
  water += 1-0 = 1, move L→

...and so on

Final answer: 6
```

## Alternative Approaches

### Prefix/Suffix Arrays (O(n) space)
```python
def trap(self, height: List[int]) -> int:
    n = len(height)
    left_max = [0] * n
    right_max = [0] * n
    
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    
    right_max[n-1] = height[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    
    return sum(min(left_max[i], right_max[i]) - height[i] for i in range(n))
```

### Monotonic Stack (O(n) space)
Process horizontally, layer by layer, using a decreasing stack.

## Related Problems

- [[solutions/two-pointers/container-with-most-water]] - Similar two-pointer, but max area
- [[solutions/stack-queue/largest-rectangle-in-histogram]] - Monotonic stack approach

## Techniques Used

- [[techniques/two-pointers]]
- [[notes/techniques/greedy-comparison]]
