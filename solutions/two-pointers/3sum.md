---
tags:
  - difficulty/medium
  - technique/two-pointers
  - technique/sorting
  - pattern/k-sum
  - source/leetcode
  - status/solved
---

# 3Sum

**Source:** LeetCode #15  
**Difficulty:** Medium  
**Date Solved:** 2024-12-28

## Problem Statement

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

The solution set must not contain duplicate triplets.

## Approach

**Primary Technique:** [[notes/techniques/two-pointers]]  
**Secondary Techniques:** Sorting, Duplicate Skipping

### Key Insight

**Reduce 3Sum to Two Sum II**: For each element `nums[i]`, find two numbers that sum to `-nums[i]` using two pointers on the remaining sorted array.

### Duplicate Handling

To avoid duplicate triplets:
1. **Skip duplicate `i`**: If `nums[i] == nums[i-1]`, we've already processed this value
2. **Skip duplicate `l` and `r`**: After finding a valid triplet, skip same values

## Solution

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        n = len(nums)

        for i in range(n - 2):
            # Skip duplicate i values
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            l, r = i + 1, n - 1

            while l < r:
                total = nums[i] + nums[l] + nums[r]

                if total == 0:
                    res.append([nums[i], nums[l], nums[r]])

                    # Skip duplicate l values
                    while l < r and nums[l] == nums[l + 1]:
                        l += 1
                    # Skip duplicate r values
                    while l < r and nums[r] == nums[r - 1]:
                        r -= 1

                    l += 1
                    r -= 1

                elif total < 0:
                    l += 1
                else:
                    r -= 1

        return res
```

## Complexity Analysis

- **Time Complexity:** O(n²) — O(n log n) sort + O(n) × O(n) two-pointer passes
- **Space Complexity:** O(1) — excluding output array (O(k) for k triplets)

## Visual Walkthrough

```
nums = [-1, 0, 1, 2, -1, -4]
sorted = [-4, -1, -1, 0, 1, 2]

i=0: nums[i]=-4, target=4
     l=1, r=5: -1+2=1 < 4 → l++
     l=2, r=5: -1+2=1 < 4 → l++
     l=3, r=5: 0+2=2 < 4 → l++
     l=4, r=5: 1+2=3 < 4 → l++
     l=5, r=5: done

i=1: nums[i]=-1, target=1
     l=2, r=5: -1+2=1 ✓ → append [-1,-1,2], skip dups
     l=3, r=4: 0+1=1 ✓ → append [-1,0,1]
     l=4, r=3: done

i=2: nums[i]=-1, SKIP (duplicate of i=1)

i=3: nums[i]=0, target=0
     l=4, r=5: 1+2=3 > 0 → r--
     l=4, r=4: done

Result: [[-1,-1,2], [-1,0,1]]
```

## Key Insights

1. **Sorting is essential** — enables two pointers AND makes duplicates adjacent
2. **Two levels of duplicate skipping**:
   - Outer loop: `if nums[i] == nums[i-1]: continue`
   - Inner loop: Skip same `l` and `r` after finding a triplet
3. **Early termination**: If `nums[i] > 0`, no valid triplet exists (all positive sum > 0)
4. **k-Sum pattern**: This generalizes to 4Sum, 5Sum, etc.

## Common Mistakes

- Forgetting to skip duplicates → duplicate triplets in result
- Wrong duplicate check: `nums[i] == nums[i+1]` vs `nums[i] == nums[i-1]`
- Not moving both `l` and `r` after finding a match

## Optimization: Early Exit

```python
# Add after the duplicate check for i
if nums[i] > 0:
    break  # No triplet possible with all positive numbers
```

## Related Problems

- [[solutions/two-pointers/two-sum-ii]] - Base pattern (two pointers)
- [[solutions/arrays/two-sum]] - Unsorted version with hashmap
- [[solutions/two-pointers/4sum]] - Extension to four elements

## Techniques Used

- [[notes/techniques/two-pointers]] - Inward pointers for pair finding
- [[notes/techniques/sorting-problems]] - Sort to enable two pointers

## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-12-28 | 4/5 | Understood duplicate handling, core pattern solid |
