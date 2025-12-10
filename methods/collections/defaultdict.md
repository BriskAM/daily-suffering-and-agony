---
tags:
  - method/collections
  - python
  - data-structure/hashmap
---

# defaultdict

**Module:** `collections`  
**Import:** `from collections import defaultdict`

## What is it?

A dict subclass that calls a factory function to supply missing values. Eliminates the need to check if a key exists before using it.

## When to Use

- Grouping elements by key
- Building adjacency lists for graphs
- Counting with nested structures
- Any time you'd write `if key not in dict: dict[key] = default`

## Syntax and Examples

```python
from collections import defaultdict

# List as default (grouping)
groups = defaultdict(list)
groups['a'].append(1)  # No KeyError, creates list automatically
groups['a'].append(2)
# defaultdict(list, {'a': [1, 2]})

# Int as default (counting)
counts = defaultdict(int)
counts['a'] += 1       # No KeyError, starts at 0
# defaultdict(int, {'a': 1})

# Set as default (unique grouping)
unique_groups = defaultdict(set)
unique_groups['a'].add(1)

# Nested defaultdict (2D grouping)
nested = defaultdict(lambda: defaultdict(list))
nested['row1']['col1'].append('value')
```

## Common Operations

| Factory | Default Value | Use Case |
|---------|---------------|----------|
| `list` | `[]` | Grouping elements |
| `int` | `0` | Counting |
| `set` | `set()` | Unique grouping |
| `lambda: defaultdict(...)` | Nested dict | Multi-level grouping |

## DSA Patterns

```python
# Grouping pattern
def group_by_key(items, key_func):
    groups = defaultdict(list)
    for item in items:
        groups[key_func(item)].append(item)
    return dict(groups)

# Graph adjacency list
def build_graph(edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)  # undirected
    return graph
```

## Problems Using This Method

- [[solutions/arrays/group-anagrams]] - Group strings by anagram key

## Related Methods

- [[methods/built-ins/dict]] - Base dict, requires key checks
- [[methods/collections/counter]] - Specialized for counting
