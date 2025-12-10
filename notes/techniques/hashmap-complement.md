---
tags:
  - technique/hashmap
  - pattern/complement
---

# HashMap Complement Pattern

## What is it?

A technique where you use a hashmap to find pairs/tuples that satisfy a mathematical relationship. Instead of brute-forcing all combinations, you store seen elements and check if their "complement" exists.

## When to Use

- Finding pairs that sum to a target
- Finding pairs with a specific difference
- Any problem asking "find two elements where f(a, b) = target"
- Key indicator: "return indices" or "return elements" of a pair

## Template/Pattern

```python
def find_pair(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num  # or any other relationship
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

## Problems Using This Technique

- [[solutions/arrays/two-sum]] - Classic complement sum problem

## Related Techniques

- [[techniques/two-pointers]] - Alternative for sorted arrays
- [[techniques/sliding-window]] - For contiguous subarray problems

## Tips and Pitfalls

- **Check complement BEFORE adding current element** to avoid using same element twice
- Use `in dict` not `in dict.keys()` - same result but cleaner
- For problems asking "count pairs", you may need to handle duplicates differently
- Consider edge cases: empty array, single element, no solution
