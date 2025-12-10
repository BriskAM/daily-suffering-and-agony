---
tags:
  - technique/stack
  - pattern/auxiliary-structure
---

# Auxiliary Stack Pattern

## What is it?

Using a secondary (auxiliary) stack to keep track of metadata, aggregates, or state related to the main data structure (usually another stack).

## When to Use

- "Retrieve minimum/maximum element in O(1)"
- "Retrieve average/sum in O(1)" (though simple variable might suffice)
- Tracking history of state changes (undo/redo with state snapshots)
- Solving problems where the "monotonic" property is needed for range queries on the stack

## Template/Pattern

### 1. 1-to-1 Mapping (Simpler Logic)

Every element in the main stack has a corresponding element in the auxiliary stack representing the state *at that height*.

```python
class EnhancedStack:
    def __init__(self):
        self.stack = []
        self.aux = []  # Tracks min/max/etc.

    def push(self, x):
        self.stack.append(x)
        if not self.aux:
            self.aux.append(x)
        else:
            # E.g., for Min Stack
            current_val = self.aux[-1]
            self.aux.append(min(x, current_val))

    def pop(self):
        self.stack.pop()
        self.aux.pop()  # Always pop both

    def get_meta(self):
        return self.aux[-1]
```

### 2. Optimized Mapping (Space Efficient)

Only push to auxiliary stack when the state *changes* or matches the condition of interest.

```python
# Min Stack Optimized
def push(self, x):
    self.stack.append(x)
    # Only push if x is new min (or equal to handle duplicates)
    if not self.min_stack or x <= self.min_stack[-1]:
        self.min_stack.append(x)

def pop(self):
    val = self.stack.pop()
    # Only pop min if we are removing the current min
    if val == self.min_stack[-1]:
        self.min_stack.pop()
```

## Common Use Cases

| Problem Type | Auxiliary Content | Notes |
|--------------|-------------------|-------|
| **Min Stack** | Minimum value seen so far | O(1) `getMin()` |
| **Max Stack** | Maximum value seen so far | O(1) `getMax()` |
| **Max Frequency** | Frequency of elements | See "Freq Stack" |
| **Undo Operations** | Previous states | Keep history of changes |

## Tips and Pitfalls

- **Synchronization:** Vital to keep `stack` and `min_stack` synchronized (or logic consistent) on pop.
- **Empty Stack Exception:** Always check if stack is empty before accessing logic that depends on `top`.
- **Initialization:** Be careful with the first element pushed (auxiliary stack is empty).
- **Equal Values:** When using the "Optimized" approach, failing to push *equal* values to `min_stack` will break `getMin` when the first instance of that min is popped but others remain. ALWAYS use `<=`.

## Problems Using This Technique

- [[solutions/stack-queue/min-stack]] - Classic example

## Related Techniques

- [[techniques/monotonic-stack]] - Specialized version where stack itself maintains order
- [[techniques/stack-matching]] - Using stack for validity checks
