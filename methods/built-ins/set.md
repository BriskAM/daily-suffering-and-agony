---
tags:
  - method/built-in
  - python
  - data-structure/hashset
---

# set

**Module:** Built-in  
**Import:** No import needed

## What is it?

An unordered collection of unique elements with O(1) average-case lookup, insertion, and deletion. Perfect for membership testing and duplicate elimination.

## When to Use

- Checking if element exists (membership test)
- Removing duplicates from a collection
- Set operations (union, intersection, difference)
- Tracking "seen" elements

## Syntax and Examples

```python
# Creation
s = set()                    # Empty set
s = {1, 2, 3}               # Literal (not {} - that's dict)
s = set([1, 2, 2, 3])       # From iterable, removes dupes -> {1, 2, 3}

# Operations
s.add(4)                     # Add element
s.remove(4)                  # Remove (raises KeyError if missing)
s.discard(4)                 # Remove (no error if missing)
s.pop()                      # Remove and return arbitrary element

# Membership
if x in s:                   # O(1) lookup
    pass

# Set operations
a | b                        # Union
a & b                        # Intersection
a - b                        # Difference
a ^ b                        # Symmetric difference
```

## Common Operations

| Operation | Syntax | Time | Description |
|-----------|--------|------|-------------|
| Add | `s.add(x)` | O(1) | Add element |
| Remove | `s.remove(x)` | O(1) | Remove (error if missing) |
| Discard | `s.discard(x)` | O(1) | Remove (no error) |
| Check | `x in s` | O(1) | Membership test |
| Union | `a \| b` | O(len(a)+len(b)) | Combine sets |
| Intersection | `a & b` | O(min(len(a),len(b))) | Common elements |

## DSA Patterns

```python
# Duplicate detection
def has_duplicate(nums):
    seen = set()
    for n in nums:
        if n in seen:
            return True
        seen.add(n)
    return False

# Remove duplicates (preserving order)
def remove_dupes(nums):
    seen = set()
    result = []
    for n in nums:
        if n not in seen:
            seen.add(n)
            result.append(n)
    return result

# Quick duplicate check
def has_duplicate_oneliner(nums):
    return len(nums) != len(set(nums))
```

## Problems Using This Method

- [[solutions/arrays/contains-duplicate]] - Duplicate detection with set

## Related Methods

- [[methods/built-ins/dict]] - When you need key-value pairs
- [[methods/collections/Counter]] - When you need frequency counts
