---
tags:
  - difficulty/medium
  - technique/stack
  - pattern/auxiliary-stack
  - company/amazon
  - company/bloomberg
  - company/uber
  - status/solved
---

# Min Stack

**Source:** LeetCode #155  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

**Constraints:**
- Methods `pop`, `top`, and `getMin` operations will always be called on non-empty stacks.
- All operations must be O(1) time complexity.

**Example:**
```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]
```

## Approach

**Primary Technique:** [[notes/techniques/auxiliary-stack]]

To achieve O(1) `getMin()`, we cannot iterate through the stack. We need to store the minimum state alongside the data.

**Strategy:** Use two stacks.
1. `stack`: Stores the actual values.
2. `min_stack`: Stores the minimum value encountered *at each level* of the main stack.

When pushing `val`:
- Push `val` to `stack`.
- Push `min(val, min_stack.top)` to `min_stack` (or just `val` if it's smaller/equal to current min).

When popping:
- Pop from both `stack` and `min_stack`.

The `min_stack` always has the minimum of the `stack` up to that height at its top.

## Solution

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []
        
    def push(self, val: int) -> None:
        self.stack.append(val)
        # If min_stack is empty or val is smaller/equal to current min, push it
        # Note: We must push even if equal to handle duplicates correctly on pop
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        else:
            # Optimization: If val > min, we don't strictly need to push to min_stack
            # IF we only push when val <= min.
            # However, the user's provided solution duplicates the previous min!
            # Let's adjust explanation to match the provided code's logic (Duplicate Min approach)
            self.min_stack.append(self.min_stack[-1])

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

## Complexity Analysis

- **Time Complexity:** O(1) for all operations (`push`, `pop`, `top`, `getMin`).
- **Space Complexity:** O(n) - Two stacks, each storing `n` elements.

## Alternative Approaches

### 1. Optimized Storage (Space Efficient)
Instead of pushing to `min_stack` every time, only push when `val <= current_min`. When popping, only pop from `min_stack` if `stack.pop() == min_stack.top()`.
- **Pros:** Saves space if duplicates/larger numbers are common.
- **Cons:** Slightly more conditional logic. The provided solution uses the "1-to-1 mapping" approach which is simpler but uses strictly 2N space (or N space if we consider the MinStack object itself).

### 2. Storing Tuples
Store `(val, current_min)` tuples in a single stack.
- `stack = [(val1, min1), (val2, min2), ...]`
- **Pros:** Single list object.
- **Cons:** Still O(n) space.

### 3. Linked List Node
Create a Node class `Node(val, min_val, next)`.
- **Pros:** flexible.
- **Cons:** Python overhead for objects.

## Visualization

**Operations:** `push(-2), push(0), push(-3), pop()`

```
1. Push -2
   Stack:     [-2]
   MinStack:  [-2]

2. Push 0
   Stack:     [-2, 0]
   MinStack:  [-2, -2]  (0 > -2, so copy -2)

3. Push -3
   Stack:     [-2, 0, -3]
   MinStack:  [-2, -2, -3] (-3 < -2, so push -3)

4. Pop
   Stack:     [-2, 0]     (removed -3)
   MinStack:  [-2, -2]    (removed -3)
   Current Min: -2
```

## Related Problems

- [[solutions/stack-queue/valid-parentheses]] - Basic stack usage
- [[solutions/heap/sliding-window-maximum]] - "Monotonic Queue" for sliding window min/max

## Techniques Used

- [[notes/techniques/auxiliary-stack]]
- [[notes/techniques/stack-matching]]
