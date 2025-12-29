---
tags:
  - technique/two-pointers
  - category/arrays
  - category/strings
---

# Two Pointers

## What is it?

The **two pointers** technique uses two index variables that move through a data structure (usually an array or string) in a coordinated way. The pointers can:
- Start at opposite ends and move inward
- Start together and move at different speeds (fast/slow)
- Start at different positions and move same direction

This technique often reduces O(n²) brute force to O(n).

## When to Use

- **Sorted arrays** - searching for pairs with a target sum
- **Palindrome checking** - compare characters from both ends
- **Removing duplicates** - in-place with fast/slow pointers
- **Partitioning** - separating elements by some condition
- **Merging** - combining two sorted arrays
- **Sliding window problems** - often use left/right pointers
- **Key indicator:** "Find a pair", "Compare from ends", "In-place modification"

## Template/Pattern

### Opposite Direction (Inward)

```python
def two_pointers_inward(arr):
    left, right = 0, len(arr) - 1
    
    while left < right:
        # Process arr[left] and arr[right]
        if some_condition:
            left += 1
        else:
            right -= 1
    
    return result
```

### Same Direction (Fast/Slow)

```python
def two_pointers_same_direction(arr):
    slow = 0
    
    for fast in range(len(arr)):
        if should_keep(arr[fast]):
            arr[slow] = arr[fast]
            slow += 1
    
    return slow  # new length
```

### Two Sum Pattern (Sorted Array)

```python
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    
    while left < right:
        curr_sum = arr[left] + arr[right]
        if curr_sum == target:
            return [left, right]
        elif curr_sum < target:
            left += 1  # need larger sum
        else:
            right -= 1  # need smaller sum
    
    return []
```

## Problems Using This Technique

- [[solutions/two-pointers/valid-palindrome]] - Compare from both ends
- [[solutions/two-pointers/two-sum-ii]] - Sorted array pair sum
- [[solutions/two-pointers/3sum]] - Extension of two sum
- [[solutions/two-pointers/container-with-most-water]] - Area maximization
- [[solutions/two-pointers/trapping-rain-water]] - Water between bars

## Related Techniques

- [[techniques/sliding-window]] - Often uses two pointers for window bounds
- [[techniques/fast-slow-pointers]] - Cycle detection variant

## Tips & Pitfalls

- **Sorted requirement**: Many two-pointer solutions require sorted input
- **Pointer movement**: Think carefully about when to move which pointer
- **Off-by-one**: Be careful with `left < right` vs `left <= right`
- **Skip duplicates**: For problems like 3Sum, skip duplicate values to avoid repeated answers
- **In-place modification**: Use slow pointer as "write position"
