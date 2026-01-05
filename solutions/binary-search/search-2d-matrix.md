# Search a 2D Matrix

**Source:** LeetCode #74  
**Difficulty:** Medium  
**Date Solved:** 2026-01-03

## Problem Statement

Given an `m x n` matrix where each row is sorted and the first element of each row is greater than the last element of the previous row, determine if a target value exists.

## Approach

**Primary Technique:** [[notes/techniques/binary-search]]

Treat the 2D matrix as a flattened 1D sorted array:
- Total elements: `m * n`
- Index `i` maps to `matrix[i // n][i % n]`

Apply standard binary search on indices `[0, m*n - 1]`.

## Solution

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        left, right = 0, m * n - 1
        
        while left <= right:
            mid = (left + right) // 2
            row = mid // n
            col = mid % n
            
            val = matrix[row][col]
            
            if val == target:
                return True
            elif val < target:
                left = mid + 1
            else:
                right = mid - 1
        
        return False
```

## Complexity Analysis

- **Time Complexity:** O(log(m*n)) = O(log m + log n)
- **Space Complexity:** O(1)

## Key Insights

1. **Index mapping:** `row = mid // n`, `col = mid % n`
2. **No actual flattening:** We just compute indices on the fly
3. **Condition:** Each row's first > previous row's last makes this work

## Alternative: Two Binary Searches

```python
def searchMatrix(self, matrix, target):
    # Binary search for row
    rows = len(matrix)
    lo, hi = 0, rows - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if matrix[mid][0] <= target <= matrix[mid][-1]:
            # Binary search within row
            row = matrix[mid]
            l, r = 0, len(row) - 1
            while l <= r:
                m = (l + r) // 2
                if row[m] == target:
                    return True
                elif row[m] < target:
                    l = m + 1
                else:
                    r = m - 1
            return False
        elif matrix[mid][0] > target:
            hi = mid - 1
        else:
            lo = mid + 1
    return False
```

## Related Problems

- [[solutions/binary-search/binary-search]] - Base technique
- [[solutions/binary-search/search-2d-matrix-ii]] - Different matrix property

## Techniques Used

- [[notes/techniques/binary-search]]
