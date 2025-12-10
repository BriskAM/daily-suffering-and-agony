---
tags:
  - technique/hashset
  - pattern/duplicate-detection
---

# HashSet Lookup Pattern

## What is it?

Using a set to track seen elements for O(1) membership testing. The fundamental pattern for duplicate detection and existence checking.

## When to Use

- Detecting duplicates in an array
- Checking if element was seen before
- Finding missing/extra elements
- Any "exists in collection" query

## Template/Pattern

```python
def detect_duplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True  # or handle duplicate
        seen.add(num)
    return False

# One-liner alternative (processes all elements)
def has_duplicate(nums):
    return len(nums) != len(set(nums))
```

## Problems Using This Technique

- [[solutions/arrays/contains-duplicate]] - Basic duplicate detection
- [[solutions/arrays/longest-consecutive-sequence]] - Sequence detection with O(n) optimization

## Advanced Pattern: Sequence Start Optimization

For problems involving sequences, only process elements that are the **start** of a sequence:

```python
# Find longest consecutive sequence
nums_set = set(nums)
for num in nums_set:
    if num - 1 not in nums_set:  # Only start from sequence beginning
        # Count sequence length from here
        length = 1
        while num + 1 in nums_set:
            length += 1
            num += 1
```

**Why this matters:** Without the `num - 1` check, you'd recount elements in the same sequence multiple times, leading to O(n²) instead of O(n).

## Related Techniques

- [[techniques/hashmap-complement]] - When you need to store values with keys
- [[techniques/frequency-count]] - When you need to count occurrences

## Tips and Pitfalls

- Use `set()` not `{}` for empty set (`{}` creates empty dict)
- Early return is more efficient than processing entire array
- Sets only work with hashable (immutable) elements
- For lists of lists, convert inner lists to tuples first
