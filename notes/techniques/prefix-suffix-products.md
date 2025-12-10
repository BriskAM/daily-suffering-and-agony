---
tags:
  - technique/prefix-suffix
  - pattern/preprocessing
---

# Prefix-Suffix Products

## What is it?

A technique that decomposes array problems into two components: prefix (everything before index i) and suffix (everything after index i). By computing both, you can efficiently answer queries about "all elements except current."

**Core Idea:** `answer[i] = prefix[i] × suffix[i]`

## When to Use

- "Product/sum of all except current element"
- "Maximum to the left and right"
- Problems that require O(n) time without division
- Questions about "everything before" and "everything after"
- Building auxiliary information for range queries

## Template/Pattern

```python
def prefix_suffix_template(nums):
    n = len(nums)
    result = [1] * n
    
    # Build prefix (left to right)
    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]  # or use other operation
    
    # Build suffix (right to left) and combine
    suffix = 1
    for i in range(n - 1, -1, -1):
        result[i] *= suffix  # combine with prefix
        suffix *= nums[i]  # or use other operation
    
    return result
```

## Common Variations

### 1. Separate Prefix/Suffix Arrays (O(n) space)

```python
# Clearer but uses more space
prefix = [1] * n
suffix = [1] * n

for i in range(1, n):
    prefix[i] = prefix[i-1] * nums[i-1]

for i in range(n-2, -1, -1):
    suffix[i] = suffix[i+1] * nums[i+1]

result = [prefix[i] * suffix[i] for i in range(n)]
```

### 2. Prefix Sum (for addition)

```python
# Running sum from left and right
def prefix_suffix_sum(nums):
    n = len(nums)
    prefix_sum = [0] * n
    suffix_sum = [0] * n
    
    prefix_sum[0] = nums[0]
    for i in range(1, n):
        prefix_sum[i] = prefix_sum[i-1] + nums[i]
    
    suffix_sum[n-1] = nums[n-1]
    for i in range(n-2, -1, -1):
        suffix_sum[i] = suffix_sum[i+1] + nums[i]
    
    return prefix_sum, suffix_sum
```

### 3. Prefix Max/Min

```python
# Maximum element to the left and right
def prefix_suffix_max(nums):
    n = len(nums)
    left_max = [0] * n
    right_max = [0] * n
    
    left_max[0] = nums[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], nums[i])
    
    right_max[n-1] = nums[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], nums[i])
    
    return left_max, right_max
```

## Visual Example

**Problem:** Product of array except self  
**Input:** `[2, 3, 4, 5]`

```
Prefix products:  [1,   2,   6,   24]
                   ↑    ↑    ↑     ↑
Index:             0    1    2     3
Numbers:          [2,   3,   4,    5]
                   ↓    ↓    ↓     ↓
Suffix products:  [60,  20,  5,    1]

Result: prefix × suffix
        [60,  40,  30,   24]
```

## Key Advantages

| Advantage | Explanation |
|-----------|-------------|
| O(n) time | Two passes through array |
| O(1) space | Can build in-place using output array |
| No division | Avoids division edge cases (zeros, overflow) |
| Composable | Combine different operations (product, sum, max) |

## Problems Using This Technique

- [[solutions/arrays/product-except-self]] - Product of all except current

## Related Techniques

- [[techniques/sliding-window]] - Similar two-pointer concept
- [[techniques/two-pointers]] - Bidirectional processing
- [[techniques/prefix-sum]] - Specialized for sums

## Tips and Pitfalls

- **Initialize correctly:** Prefix starts at 1 for products, 0 for sums
- **Index management:** Prefix uses `i`, suffix often uses `n-1-i` or reverse loop
- **Update order:** Store prefix first, THEN update running value
- **Space optimization:** Use output array to store prefix, update in-place with suffix
- **Edge cases:** Single element, all zeros, empty array
- **Operation matters:** Product vs sum vs max have different neutral elements

## Real-World Applications

- **Stock trading:** Maximum profit with transactions before and after each day
- **Water trapping:** Height of water based on tallest bars left and right
- **Array statistics:** Compute metrics excluding current element
- **Game theory:** Best move considering all other players
