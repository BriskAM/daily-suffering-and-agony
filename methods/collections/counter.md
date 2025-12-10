---
tags:
  - method/collections
  - python
  - data-structure/hashmap
---

# Counter

**Module:** `collections`  
**Import:** `from collections import Counter`

## What is it?

A dict subclass for counting hashable objects. Elements are stored as dictionary keys and their counts are stored as dictionary values. The most Pythonic way to count frequencies.

## When to Use

- Counting character frequencies in strings
- Counting element frequencies in arrays
- Finding most/least common elements
- Comparing frequency distributions (anagrams)
- Histogram-style problems

## Syntax and Examples

```python
from collections import Counter

# Creation
c = Counter()                    # Empty counter
c = Counter("aabbc")             # From string -> {'a': 2, 'b': 2, 'c': 1}
c = Counter([1, 1, 2, 3, 3, 3])  # From list -> {3: 3, 1: 2, 2: 1}
c = Counter({'a': 2, 'b': 1})    # From dict

# Access
c['a']                           # Get count (returns 0 for missing, not KeyError)
c.most_common(2)                 # Top 2: [('a', 2), ('b', 2)]
c.most_common()                  # All, sorted by frequency

# Modification
c['a'] += 1                      # Increment
c.update("aaa")                  # Add counts from iterable
c.subtract("a")                  # Subtract counts

# Arithmetic
c1 + c2                          # Add counts
c1 - c2                          # Subtract (keeps only positive)
c1 & c2                          # Intersection (min of counts)
c1 | c2                          # Union (max of counts)
```

## Common Operations

| Operation | Syntax | Description |
|-----------|--------|-------------|
| Count elements | `Counter(iterable)` | Create frequency map |
| Get count | `c[key]` | Returns 0 if missing |
| Most common | `c.most_common(n)` | Top n by frequency |
| Total count | `sum(c.values())` | Sum of all counts |
| Unique elements | `list(c.keys())` | All unique elements |
| Add counts | `c.update(iterable)` | Increment counts |
| Compare | `c1 == c2` | True if same frequencies |

## DSA Patterns

```python
# Character frequency
freq = Counter(s)

# Check anagram
def is_anagram(s, t):
    return Counter(s) == Counter(t)

# Top K frequent
def top_k(nums, k):
    return [x for x, _ in Counter(nums).most_common(k)]

# Check if can construct
def can_construct(ransomNote, magazine):
    return Counter(ransomNote) <= Counter(magazine)
```

## Problems Using This Method

- [[solutions/arrays/valid-anagram]] - Compare character frequencies
- [[solutions/arrays/top-k-frequent-elements]] - Use most_common() for top k elements
- [[solutions/arrays/valid-sudoku]] - Detect duplicate digits in rows/columns/boxes

## Related Methods

- [[methods/built-ins/dict]] - Base class, manual counting
- [[methods/collections/defaultdict]] - Auto-initialize missing keys
- [[methods/built-ins/set]] - When you only need uniqueness
