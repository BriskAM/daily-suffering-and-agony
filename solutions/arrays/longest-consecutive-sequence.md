---
tags:
  - difficulty/medium
  - technique/hashset-lookup
  - pattern/sequence
  - company/google
  - company/facebook
  - company/amazon
  - status/solved
  - optimization/sequence-start
---

# Longest Consecutive Sequence

**Source:** LeetCode #128  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

**Constraint:** Must run in O(n) time.

**Example:**
```
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: Longest consecutive sequence is [1, 2, 3, 4]
```

## Approach

**Primary Technique:** [[notes/techniques/hashset-lookup]]

**Key Insight:** Only start counting from elements that are the **beginning of a sequence** (i.e., `num - 1` not in set). This ensures each element is visited at most twice, achieving O(n) time.

## Solution

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums_set = set(nums)
        seq = 0
        
        for num in nums_set:
            # Only start sequence if this is the beginning
            if num - 1 not in nums_set:
                length = 1
                curr = num
                
                # Count consecutive elements
                while curr + 1 in nums_set:
                    length += 1
                    curr += 1
                
                seq = max(length, seq)
        
        return seq
```

## Complexity Analysis

- **Time Complexity:** O(n)
  - Convert to set: O(n)
  - Iterate through unique numbers: O(n)
  - While loop: Each number visited at most once across all iterations
  - Total: O(n)
- **Space Complexity:** O(n) for the set

## Why This is O(n), Not O(n²)

### ❌ Without Sequence Start Check (O(n²))

```python
# WRONG - This would be O(n²)
def longestConsecutive(nums):
    nums_set = set(nums)
    seq = 0
    
    for num in nums_set:
        # Start from EVERY number (mistake!)
        length = 1
        curr = num
        while curr + 1 in nums_set:
            length += 1
            curr += 1
        seq = max(length, seq)
    
    return seq
```

**Why it's slow:**
- For sequence `[1, 2, 3, 4, 5]`
  - Starting from 1: counts 5 elements
  - Starting from 2: counts 4 elements  
  - Starting from 3: counts 3 elements
  - Starting from 4: counts 2 elements
  - Starting from 5: counts 1 element
  - **Total:** 5 + 4 + 3 + 2 + 1 = 15 operations = O(n²)

### ✅ With Sequence Start Check (O(n))

```python
# CORRECT - This is O(n)
if num - 1 not in nums_set:  # Only start from sequence beginning
    # count sequence...
```

**Why it's fast:**
- For sequence `[1, 2, 3, 4, 5]`
  - 1 is start (`0` not in set): counts 5 elements
  - 2 is NOT start (`1` in set): skipped
  - 3 is NOT start (`2` in set): skipped
  - 4 is NOT start (`3` in set): skipped
  - 5 is NOT start (`4` in set): skipped
  - **Total:** 5 operations = O(n)

**Key:** Each element is counted exactly once across all iterations!

## Visualization

```
Input: [100, 4, 200, 1, 3, 2]
Set: {1, 2, 3, 4, 100, 200}

Iteration:
  1: (1-1=0 not in set) ✓ Start sequence → 1,2,3,4 → length=4
  2: (2-1=1 in set) ✗ Skip (not a start)
  3: (3-1=2 in set) ✗ Skip (not a start)
  4: (4-1=3 in set) ✗ Skip (not a start)
100: (100-1=99 not in set) ✓ Start sequence → 100 → length=1
200: (200-1=199 not in set) ✓ Start sequence → 200 → length=1

Max length: 4
```

## How It Works

1. **Convert to set:** O(1) lookups for `num - 1` and `num + 1`
2. **Iterate unique numbers:** Avoid duplicate work
3. **Check sequence start:** `if num - 1 not in nums_set`
   - If `num - 1` exists, `num` is in the middle/end of a sequence
   - Skip it because we'll count it when we hit the sequence start
4. **Count from start:** Use while loop to find consecutive numbers
5. **Track maximum:** Update global max length

## Edge Cases Handled

| Case | Input | Output | Notes |
|------|-------|--------|-------|
| Empty array | `[]` | `0` | No sequences |
| Single element | `[1]` | `1` | One element sequence |
| No consecutive | `[1, 3, 5]` | `1` | All separate |
| All consecutive | `[1,2,3,4]` | `4` | One full sequence |
| Duplicates | `[1,2,2,3]` | `3` | Set removes dups |
| Negative numbers | `[-1,0,1]` | `3` | Works with negatives |

## Alternative Approaches

### Sorting (O(n log n))

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    nums.sort()
    longest = 1
    current = 1
    
    for i in range(1, len(nums)):
        if nums[i] == nums[i-1]:  # Skip duplicates
            continue
        elif nums[i] == nums[i-1] + 1:  # Consecutive
            current += 1
        else:  # Break in sequence
            longest = max(longest, current)
            current = 1
    
    return max(longest, current)
```

**Trade-off:** Simpler logic but O(n log n) time due to sorting. Doesn't meet O(n) constraint.

## Key Insights

- **Set for O(1) lookup:** Check `num ± 1` in constant time
- **Sequence start check:** Critical optimization from O(n²) to O(n)
- **Each element counted once:** Amortized O(1) per element
- **Skip middle elements:** Don't re-count elements in same sequence
- **Duplicates handled:** Set automatically deduplicates

## Related Problems

- [[solutions/arrays/contains-duplicate]] - Basic set usage
- [[solutions/arrays/two-sum]] - Hash-based optimization

## Techniques Used

- [[notes/techniques/hashset-lookup]]
- [[methods/built-ins/set]]
