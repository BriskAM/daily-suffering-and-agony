# Binary Search

## What is it?

A divide-and-conquer technique that repeatedly halves the search space to find a target value or boundary in O(log n) time. Works on sorted or monotonic data.

## When to Use

- Searching in a **sorted array**
- Finding a boundary (first/last occurrence, minimum satisfying condition)
- Searching on a **monotonic function** (answer space binary search)
- Any problem where you can eliminate half the search space per step

**Key indicators:**
- "Sorted array" in the problem
- O(log n) requirement
- "Find minimum/maximum X such that condition"
- Search space can be halved based on a condition

## Template/Pattern

### Standard Binary Search (Find Exact)
```python
def binary_search(nums, target):
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

### Lower Bound (First >= Target)
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

### Upper Bound (First > Target)
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

### Binary Search on Answer
```python
def binary_search_answer(lo, hi, condition):
    while lo < hi:
        mid = (lo + hi) // 2
        if condition(mid):
            hi = mid  # mid could be answer, search left
        else:
            lo = mid + 1  # mid is not answer, search right
    return lo
```

## Critical Details

| Variant | Loop Condition | Right Init | Update |
|---------|----------------|------------|--------|
| Exact match | `left <= right` | `len-1` | `mid ± 1` |
| Lower/Upper bound | `left < right` | `len` | `right = mid` or `left = mid + 1` |

## Common Pitfalls

1. **Infinite loop:** Forgetting `+1` or `-1` in updates
2. **Wrong condition:** `<` vs `<=` in loop
3. **Off-by-one:** `right = len(nums)` vs `right = len(nums) - 1`
4. **Integer overflow:** Use `left + (right - left) // 2` in other languages

## Problems Using This Technique

- [[solutions/binary-search/binary-search]] - Classic find target
- [[solutions/binary-search/search-2d-matrix]] - Treat matrix as sorted array
- [[solutions/binary-search/search-rotated-sorted-array]] - Handle rotation
- [[solutions/binary-search/find-minimum-rotated-sorted-array]] - Find pivot point

## Related Techniques

- [[techniques/two-pointers]] - Both narrow a range
- [[notes/techniques/sorting-problems]] - Often precedes binary search

## Tips

- Draw out the search space and trace through examples
- For lower/upper bound, think "what am I looking for?" and adjust `<=` / `<`
- When in doubt, use `bisect` module in Python: `bisect.bisect_left`, `bisect.bisect_right`
