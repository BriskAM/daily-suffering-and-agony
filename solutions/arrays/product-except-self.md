---
tags:
  - difficulty/medium
  - technique/prefix-suffix
  - pattern/array-product
  - company/google
  - company/amazon
  - company/facebook
  - status/solved
  - constraint/no-division
---

# Product of Array Except Self

**Source:** LeetCode #238  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all elements of `nums` except `nums[i]`.

**Constraints:**
- Must run in O(n) time
- Cannot use division operator
- Follow-up: Solve with O(1) extra space (output array doesn't count)

**Example:**
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Explanation: [2*3*4, 1*3*4, 1*2*4, 1*2*3]
```

## Approach

**Primary Technique:** [[notes/techniques/prefix-suffix-products]]

The key insight: For each index `i`, the answer is `(product of all before i) × (product of all after i)`.

**Strategy:**
1. **First pass (left to right):** Store prefix products (all elements before i)
2. **Second pass (right to left):** Multiply by suffix products (all elements after i)

This avoids division and achieves O(1) extra space by building the result in-place.

## Solution

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        prods = [1] * n
        
        # Build prefix products
        prefix = 1
        for i in range(n):
            prods[i] = prefix
            prefix *= nums[i]
        
        # Build suffix products and multiply with prefix
        suffix = 1
        for i in range(n - 1, -1, -1):
            prods[i] *= suffix
            suffix *= nums[i]
        
        return prods
```

## Complexity Analysis

- **Time Complexity:** O(n) - Two passes through the array
- **Space Complexity:** O(1) - Only output array (doesn't count per problem), two scalar variables

## How It Works

**Example:** `nums = [1, 2, 3, 4]`

### Pass 1: Prefix Products (left to right)
```
i=0: prods[0] = 1 (no elements before)       → prods = [1, _, _, _]
i=1: prods[1] = 1 (product of [1])           → prods = [1, 1, _, _]
i=2: prods[2] = 1*2 (product of [1,2])       → prods = [1, 1, 2, _]
i=3: prods[3] = 1*2*3 (product of [1,2,3])   → prods = [1, 1, 2, 6]
```

### Pass 2: Suffix Products (right to left)
```
i=3: prods[3] *= 1 (no elements after)         → prods = [1, 1, 2, 6]
i=2: prods[2] *= 4 (product of [4])            → prods = [1, 1, 8, 6]
i=1: prods[1] *= 3*4 (product of [3,4])        → prods = [1, 12, 8, 6]
i=0: prods[0] *= 2*3*4 (product of [2,3,4])    → prods = [24, 12, 8, 6]
```

**Result:** `[24, 12, 8, 6]` ✓

## Visualization

For `nums = [a, b, c, d]`, the answer array is:

```
Index 0: [  ] × [b, c, d] = b*c*d
Index 1: [a]  × [c, d]    = a*c*d
Index 2: [a,b] × [d]      = a*b*d
Index 3: [a,b,c] × []     = a*b*c
```

## Alternative Approaches

### ❌ Naive Brute Force (Time Limit Exceeded)

```python
import math

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prods = []
        for i in range(len(nums)):
            prods.append(math.prod(nums[:i] + nums[i+1:]))
        return prods
```

**Why it fails:**
- **Time Complexity:** O(n²) - For each of n elements, we create a new list and compute its product
- **Space Complexity:** O(n) - Creating `nums[:i] + nums[i+1:]` allocates new memory
- **TLE (Time Limit Exceeded):** Doesn't meet the O(n) requirement
- **Operations per element:**
  - Slicing: O(n) to create `nums[:i] + nums[i+1:]`
  - `math.prod()`: O(n) to compute product
  - Total: O(n) per element × n elements = O(n²)

**Key lesson:** This naive approach recomputes products unnecessarily. The prefix-suffix technique eliminates redundant computation by storing cumulative products.

### Using Division (Violates Constraint)

```python
def productExceptSelf(nums):
    total = 1
    for num in nums:
        total *= num
    return [total // num for num in nums]
```

**Issue:** Division not allowed by problem constraints. Also fails if any element is 0.

### Prefix and Suffix Arrays (O(n) space)

```python
def productExceptSelf(nums):
    n = len(nums)
    prefix = [1] * n
    suffix = [1] * n
    
    for i in range(1, n):
        prefix[i] = prefix[i-1] * nums[i-1]
    
    for i in range(n-2, -1, -1):
        suffix[i] = suffix[i+1] * nums[i+1]
    
    return [prefix[i] * suffix[i] for i in range(n)]
```

**Trade-off:** Clearer logic but uses O(n) extra space.

## Edge Cases Handled

| Case | Input | Output | Notes |
|------|-------|--------|-------|
| Single zero | `[0,1,2]` | `[2,0,0]` | Prefix/suffix handles naturally |
| Multiple zeros | `[0,0,1]` | `[0,0,0]` | All products become 0 |
| Single element | `[5]` | `[1]` | No other elements |
| All ones | `[1,1,1]` | `[1,1,1]` | Identity |

## Key Insights

- **No division needed:** Prefix-suffix decomposition avoids division entirely
- **In-place result:** Use output array to store prefix, then modify with suffix
- **Two passes:** Forward for prefix, backward for suffix
- **Handles zeros:** Works correctly even with zero elements
- **Scalars only:** Only need two variables (prefix, suffix) for O(1) space

## Related Problems

- [[solutions/arrays/two-sum]] - Array traversal pattern
- [[solutions/arrays/top-k-frequent-elements]] - Array analysis

## Techniques Used

- [[notes/techniques/prefix-suffix-products]]
- [[notes/techniques/in-place-computation]]
