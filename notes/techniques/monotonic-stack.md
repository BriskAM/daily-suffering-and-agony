---
tags:
  - technique/monotonic-stack
  - category/stack
---

# Monotonic Stack

## What is it?

A **monotonic stack** is a stack that maintains elements in either strictly increasing or strictly decreasing order. When a new element would break the monotonic property, we pop elements until the property is restored.

- **Monotonic Increasing Stack:** Elements increase from bottom to top
- **Monotonic Decreasing Stack:** Elements decrease from bottom to top

This technique efficiently finds the **next greater/smaller element** for each position in O(n) time.

## When to Use

- Finding the **next greater element** (NGE) for each position
- Finding the **next smaller element** (NSE) for each position
- Finding the **previous greater/smaller element**
- Problems involving spans (stock span, histogram rectangles)
- When you need to look ahead/behind for elements with certain properties
- **Key indicator:** "Find the next element that is greater/smaller than current"

## Template/Pattern

### Next Greater Element (Decreasing Stack)

```python
def next_greater_element(arr):
    n = len(arr)
    result = [-1] * n  # -1 means no greater element found
    stack = []  # stores indices
    
    for i in range(n):
        # Pop elements smaller than current (they found their NGE)
        while stack and arr[i] > arr[stack[-1]]:
            prev = stack.pop()
            result[prev] = arr[i]  # or i - prev for distance
        stack.append(i)
    
    return result
```

### Next Smaller Element (Increasing Stack)

```python
def next_smaller_element(arr):
    n = len(arr)
    result = [-1] * n
    stack = []
    
    for i in range(n):
        while stack and arr[i] < arr[stack[-1]]:
            prev = stack.pop()
            result[prev] = arr[i]
        stack.append(i)
    
    return result
```

### Previous Greater Element (Process and then push)

```python
def prev_greater_element(arr):
    n = len(arr)
    result = [-1] * n
    stack = []
    
    for i in range(n):
        while stack and arr[stack[-1]] <= arr[i]:
            stack.pop()
        if stack:
            result[i] = arr[stack[-1]]
        stack.append(i)
    
    return result
```

## Why It Works

Each element is pushed exactly once and popped at most once, giving O(n) time complexity despite the nested while loop. The stack maintains a "waiting list" of elements that haven't found their answer yet.

## Problems Using This Technique

- [[solutions/stack-queue/daily-temperatures]] - Classic NGE with distance
- [[solutions/stack-queue/largest-rectangle-in-histogram]] - NSE from both sides for span
- [[solutions/stack-queue/next-greater-element-i]] - Basic NGE
- [[solutions/stack-queue/trapping-rain-water]] - Max from both sides
- [[solutions/stack-queue/online-stock-span]] - Previous greater element

## Related Techniques

- [[techniques/auxiliary-stack]] - General stack auxiliary patterns
- [[techniques/stack-matching]] - Bracket matching problems

## Tips & Pitfalls

- **Store indices, not values** - enables distance calculations
- **Decide increasing vs decreasing** based on what you're looking for:
  - NGE → Decreasing stack
  - NSE → Increasing stack
- **Left-to-right vs right-to-left** iteration changes if you find next vs previous
- Elements remaining in stack never found their answer (handle with default values)
- Don't confuse with [[techniques/auxiliary-stack]] which tracks additional state, not ordering
