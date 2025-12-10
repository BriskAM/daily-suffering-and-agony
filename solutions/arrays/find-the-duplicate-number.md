---
tags:
  - difficulty/medium
  - technique/cycle-detection
  - technique/floyds-tortoise-hare
  - pattern/fast-slow-pointers
  - company/amazon
  - company/microsoft
  - company/google
  - status/solved
  - constraint/constant-space
---

# Find the Duplicate Number

**Source:** LeetCode #287  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is **only one repeated number** in `nums`, return this repeated number.

You must solve the problem **without modifying the array** `nums` and uses only constant extra space.

**Example:**
```
Input: nums = [1,3,4,2,2]
Output: 2

Input: nums = [3,1,3,4,2]
Output: 3
```

## Approach 1: Frequency Count (User's Solution)

Uses a hash map (Counter) to count occurrences.

```python
from collections import Counter

class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        c = Counter(nums)
        return c.most_common(1)[0][0]
```

**Analysis:**
- **Time Complexity:** O(n)
- **Space Complexity:** O(n) - Stores counts for all numbers.
- **Constraints:** Violates the "constant extra space" constraint of the specific problem statement, but is a valid approach if space is allowed.

## Approach 2: Floyd's Tortoise and Hare (Optimal)

**Primary Technique:** [[notes/techniques/floyds-cycle-detection]]

Treat the array as a linked list where the index is the node and `nums[index]` is the `next` pointer.
- `index 0` points to `nums[0]`
- `index i` points to `nums[i]`

Since values are in `[1, n]`, every value points to a valid index. The duplicate number acts as the entry point to a cycle (multiple nodes point to the same index).

**Algorithm:**
1. **Detect Cycle:** Use `slow` and `fast` pointers. `slow` moves 1 step, `fast` moves 2 steps. They will meet inside the cycle.
2. **Find Entrance:** Reset `slow` to start (0). Move both `slow` and `fast` 1 step at a time. The point where they meet is the duplicate number (cycle entrance).

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # Phase 1: Detect cycle
        slow, fast = 0, 0
        while True:
            slow = nums[slow]           # Move 1 step
            fast = nums[nums[fast]]     # Move 2 steps
            if slow == fast:
                break
        
        # Phase 2: Find cycle entrance
        slow2 = 0
        while True:
            slow = nums[slow]
            slow2 = nums[slow2]
            if slow == slow2:
                return slow
```

**Analysis:**
- **Time Complexity:** O(n)
- **Space Complexity:** O(1) - Only two pointers.
- **Constraints:** Satisfies all problem constraints (no modification, constant space).

## Why It Works

If there is a duplicate number `target`:
- Multiple positions `i` and `j` have `nums[i] == nums[j] == target`.
- This means multiple nodes point to node `target`.
- In a "linked list" graph, two incoming edges to a node create a cycle.
- The entrance to the cycle is precisely the duplicate value.

## Visualization

`nums = [1, 3, 4, 2, 2]`
Mapping: `0->1, 1->3, 2->4, 3->2, 4->2`

Graph:
`0 -> 1 -> 3 -> 2 -> 4 -> 2 (cycle back to 2)`

Cycle: `2 -> 4 -> 2`
Duplicate (Entrance): `2`

## Alternative Approaches

### Binary Search on Values (O(n log n))
Range `[1, n]`. Count how many numbers in array are `<=` `mid`. If count > `mid`, duplicate is in lower half.
- O(1) space, preserves array.
- O(n log n) time.

### Negative Marking (O(n) time, O(1) space)
Iterate array, mark `nums[abs(x)]` as negative. If already negative, `abs(x)` is duplicate.
- **Problem:** Modifies the array (violates constraint).

## Related Problems

- [[solutions/linked-lists/linked-list-cycle]] - The base technique
- [[solutions/arrays/first-missing-positive]] - Index mapping pattern

## Techniques Used

- [[notes/techniques/floyds-cycle-detection]]
- [[notes/techniques/fast-slow-pointers]]
