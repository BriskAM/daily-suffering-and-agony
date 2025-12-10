---
tags:
  - method/built-in
  - python
  - data-structure/hashmap
---

# dict (Dictionary)

**Module:** Built-in  
**Import:** No import needed

## What is it?

Python's built-in hash table implementation. Provides O(1) average-case lookup, insertion, and deletion. The workhorse data structure for DSA problems.

## When to Use

- Need fast lookups by key
- Counting frequencies
- Building mappings (value -> index, char -> count, etc.)
- Caching/memoization
- Grouping elements

## Syntax and Examples

```python
# Creation
d = {}                      # Empty dict
d = {'a': 1, 'b': 2}       # Literal
d = dict(a=1, b=2)         # Constructor

# Access and modification
d['key'] = value           # Set
value = d['key']           # Get (raises KeyError if missing)
value = d.get('key', default)  # Get with default

# Checking membership
if key in d:               # O(1) lookup
    pass

# Common operations
d.update({k: v})           # Add/update multiple
d.pop('key')               # Remove and return
d.keys()                   # All keys
d.values()                 # All values
d.items()                  # Key-value pairs
```

## Common Operations

| Operation | Syntax | Time | Description |
|-----------|--------|------|-------------|
| Get | `d[key]` or `d.get(key)` | O(1) | Retrieve value |
| Set | `d[key] = val` | O(1) | Insert/update |
| Delete | `del d[key]` or `d.pop(key)` | O(1) | Remove entry |
| Check key | `key in d` | O(1) | Membership test |
| Update | `d.update({...})` | O(k) | Merge dictionaries |

## DSA Patterns

```python
# Frequency counting
freq = {}
for x in nums:
    freq[x] = freq.get(x, 0) + 1

# Index mapping
index_map = {val: i for i, val in enumerate(nums)}

# Grouping by key
groups = {}
for item in items:
    key = get_key(item)
    if key not in groups:
        groups[key] = []
    groups[key].append(item)
```

## Problems Using This Method

- [[solutions/arrays/two-sum]] - Store seen values with indices

## Related Methods

- [[methods/collections/Counter]] - Specialized frequency counting
- [[methods/collections/defaultdict]] - Dict with default values
- [[methods/built-ins/set]] - When you only need keys, not values
