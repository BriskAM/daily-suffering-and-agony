---
tags:
  - method/built-ins
  - python
  - iteration
---

# any()

**Module:** Built-in  
**Syntax:** `any(iterable)`

## What is it?

Returns `True` if any element in the iterable is truthy. Short-circuits on first `True` value. Returns `False` for empty iterables.

## When to Use

- Check if at least one condition is met
- "Does any element satisfy...?" questions
- Early termination when searching for existence
- Validation: "Is there any violation?"
- Cleaner alternative to explicit loops with breaks

## Syntax and Examples

```python
# Basic usage
any([False, False, True, False])   # True - at least one True
any([False, False, False])         # False - all False
any([])                            # False - empty iterable

# With conditions
nums = [1, 2, 3, 4, 5]
any(x > 3 for x in nums)          # True - 4 and 5 satisfy
any(x > 10 for x in nums)         # False - none satisfy

# Short-circuiting
any([slow_check1(), slow_check2(), slow_check3()])
# Stops at first True, doesn't call remaining functions

# String checking
any(char.isdigit() for char in "abc123")  # True - has digits
any(c.isupper() for c in "hello")         # False - no uppercase
```

## Common Patterns

| Pattern | Example | Result |
|---------|---------|--------|
| Any truthy | `any([0, '', None, 1])` | `True` |
| All falsy | `any([0, '', None, False])` | `False` |
| Empty iterable | `any([])` | `False` |
| Generator | `any(x > 5 for x in range(10))` | `True` |
| Negation | `not any(...)` | Equivalent to `all(not ...)` |

## DSA Patterns

```python
# Check for duplicates
def has_duplicates(nums):
    seen = set()
    return any(num in seen or seen.add(num) for num in nums)

# Validation with Counter
from collections import Counter
count = Counter(items)
if any(c > 1 for c in count.values()):  # Any duplicates?
    return False

# Matrix validation
def has_invalid_row(matrix):
    return any(len(row) != expected_len for row in matrix)

# Search for element satisfying condition
def has_even(nums):
    return any(x % 2 == 0 for x in nums)

# Nested conditions
def has_pair_sum(matrix, target):
    return any(
        any(row[i] + row[j] == target for j in range(i+1, len(row)))
        for i, row in enumerate(matrix)
    )
```

## vs all()

| Function | Returns True when | Returns False for empty |
|----------|-------------------|------------------------|
| `any()` | At least one True | Yes (`False`) |
| `all()` | All True | No (`True`) |

```python
# Examples
any([True, False])   # True - at least one
all([True, False])   # False - not all

any([])             # False
all([])             # True (vacuous truth)
```

## Performance Notes

- **Short-circuits:** Stops at first `True` value
- **Time:** O(n) worst case, O(1) best case
- **Lazy evaluation:** Works with generators efficiently
- **Memory:** O(1) when using generators

```python
# Efficient with generators (no list creation)
any(x > 100 for x in range(1000000))  # Stops at 101

# Less efficient (creates full list first)
any([x > 100 for x in range(1000000)])
```

## Problems Using This Method

- [[solutions/arrays/valid-sudoku]] - Check if any count > 1 for duplicates

## Related Methods

- [[methods/built-ins/all]] - Check if all elements are True
- [[methods/built-ins/filter]] - Get all elements matching condition
- [[methods/built-ins/sum]] - Count True values: `sum(condition for ...)`

## Tips and Pitfalls

- **Empty iterable:** Returns `False` (different from `all()` which returns `True`)
- **Truthy values:** `any([0, '', [], {}])` is `False` - all falsy
- **Generator advantage:** Use generator expressions for memory efficiency
- **De Morgan's law:** `not any(x)` ≡ `all(not x)`
- **Side effects:** Be careful with side effects in comprehensions (e.g., `seen.add()`)
