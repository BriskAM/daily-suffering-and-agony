---
tags:
  - difficulty/medium
  - technique/frequency-count
  - technique/heap
  - pattern/top-k
  - company/google
  - company/amazon
  - status/solved
---

# Top K Frequent Elements

**Source:** LeetCode #347  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Example:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

## Approach

**Primary Technique:** [[notes/techniques/frequency-count]]  
**Secondary Techniques:** [[methods/collections/counter]], Heap (min-heap via most_common)

Use `Counter` to count frequencies, then leverage `most_common(k)` which internally uses a heap to efficiently get the top k elements.

## Solution

```python
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        counts = Counter(nums)
        return [item for item, freq in counts.most_common(k)]
```

## Complexity Analysis

- **Time Complexity:** O(n + m log k) where n is array length, m is number of unique elements
  - O(n) to build the Counter
  - O(m log k) for most_common using a heap of size k
  - In worst case where all elements are unique: O(n log k)
- **Space Complexity:** O(n) for the Counter storage

## How It Works

1. `Counter(nums)` creates a frequency map: `{1: 3, 2: 2, 3: 1}`
2. `most_common(k)` returns the k highest-frequency items as list of tuples: `[(1, 3), (2, 2)]`
3. List comprehension extracts just the items (not frequencies): `[1, 2]`

## Alternative Approaches

### Bucket Sort (O(n) time)

```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    count = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    
    for num, freq in count.items():
        buckets[freq].append(num)
    
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            return result[:k]
```

**Trade-off:** O(n) time but more complex code. Use when k is large relative to n.

## Key Insights

- `Counter.most_common(k)` is the cleanest solution for this problem
- `most_common()` internally uses `heapq.nlargest()` for efficient top-k selection
- The solution doesn't require sorting all elements, just finding top k
- Result order doesn't matter per problem constraints

## Related Problems

- [[solutions/arrays/valid-anagram]] - Uses Counter for frequency comparison
- [[solutions/arrays/group-anagrams]] - Uses Counter for grouping

## Techniques Used

- [[notes/techniques/frequency-count]]
- [[methods/collections/counter]]
