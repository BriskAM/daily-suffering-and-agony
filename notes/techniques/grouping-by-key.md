---
tags:
  - technique/grouping
  - pattern/categorization
---

# Grouping by Key Pattern

## What is it?

A pattern where elements are categorized into groups based on a computed key. All elements sharing the same key end up in the same group.

## When to Use

- "Group elements by X"
- "Find all elements that share property Y"
- Building categories or buckets
- Problems mentioning anagrams, categories, or similar items

## Template/Pattern

```python
from collections import defaultdict

def group_by(items, key_func):
    groups = defaultdict(list)
    for item in items:
        key = key_func(item)
        groups[key].append(item)
    return list(groups.values())  # or dict(groups)

# Example: group strings by length
group_by(["a", "bb", "c", "dd"], len)
# [['a', 'c'], ['bb', 'dd']]
```

## Key Functions for Common Problems

| Problem Type | Key Function |
|--------------|--------------|
| Anagrams | `''.join(sorted(s))` or `tuple(sorted(Counter(s).items()))` |
| By length | `len(s)` |
| By first char | `s[0]` |
| By remainder | `n % k` |

## Problems Using This Technique

- [[solutions/arrays/group-anagrams]] - Group by sorted string

## Related Techniques

- [[techniques/frequency-count]] - Often combined with grouping
- [[techniques/hashmap-complement]] - Different use of hashmap

## Tips and Pitfalls

- Use `defaultdict(list)` to avoid key existence checks
- Keys must be hashable (use tuple instead of list)
- Consider which key function gives better complexity
