---
tags:
  - method/built-in
  - python
---

# enumerate

**Module:** Built-in  
**Import:** No import needed

## What is it?

Returns an enumerate object that yields pairs of (index, element) from an iterable. Essential for when you need both the index and value while iterating.

## When to Use

- Need both index and value in a loop
- Tracking position while iterating
- Building index-based mappings

## Syntax and Examples

```python
# Basic usage
for i, val in enumerate(nums):
    print(f"Index {i}: {val}")

# With start parameter
for i, val in enumerate(nums, start=1):
    print(f"Position {i}: {val}")  # 1-indexed

# Convert to list of tuples
list(enumerate(['a', 'b', 'c']))  # [(0, 'a'), (1, 'b'), (2, 'c')]
```

## Common Operations

| Operation | Syntax | Description |
|-----------|--------|-------------|
| Basic enumerate | `enumerate(iterable)` | Start from 0 |
| Custom start | `enumerate(iterable, start=n)` | Start from n |
| Unpack in loop | `for i, v in enumerate(x)` | Get index and value |

## Problems Using This Method

- [[solutions/arrays/two-sum]] - Track indices while building hashmap

## Related Methods

- [[methods/built-ins/zip]] - Pair elements from multiple iterables
- [[methods/built-ins/range]] - Generate index sequences
