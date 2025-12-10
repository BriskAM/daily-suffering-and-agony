---
tags:
  - technique/frequency-count
  - pattern/counting
---

# Frequency Count Pattern

## What is it?

Counting occurrences of elements using a hashmap or Counter. Essential for problems involving distributions, anagrams, or "how many" queries.

## When to Use

- Comparing character distributions (anagrams)
- Finding most/least frequent elements
- Problems with "frequency" or "count" in description
- Checking if permutation or rearrangement is possible

## Template/Pattern

```python
from collections import Counter

# Using Counter (preferred)
def count_frequency(items):
    return Counter(items)

# Manual counting
def count_frequency_manual(items):
    freq = {}
    for item in items:
        freq[item] = freq.get(item, 0) + 1
    return freq

# Compare frequencies
def same_frequency(a, b):
    return Counter(a) == Counter(b)
```

## Problems Using This Technique

- [[solutions/arrays/valid-anagram]] - Compare character frequencies
- [[solutions/arrays/top-k-frequent-elements]] - Count frequencies and find top k elements

## Related Techniques

- [[techniques/hashset-lookup]] - When you only need existence, not count
- [[techniques/hashmap-complement]] - When looking for pairs

## Tips and Pitfalls

- Counter returns 0 for missing keys (no KeyError)
- `Counter(a) <= Counter(b)` checks if a's elements can be formed from b
- For sorted frequency: `counter.most_common()`
- Subtracting Counters drops non-positive counts
