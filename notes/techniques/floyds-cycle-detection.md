---
tags:
  - technique/two-pointers
  - pattern/cycle-detection
---

# Floyd's Cycle Detection (Tortoise and Hare)

## What is it?

A pointer algorithm that uses two pointers moving at different speeds to detect a cycle in a sequence of values (usually a linked list or array behaving like one).

**Key Idea:** If there is a cycle, the fast pointer (Hare) will eventually lap the slow pointer (Tortoise) and they will meet inside the cycle.

## When to Use

- "Check if linked list has a cycle"
- "Find the start node of a cycle"
- "Find duplicate number in array" (where array values are pointers to indices)
- Finding the middle of a linked list (fast pointer reaches end, slow is at middle)

## Algorithm Phases

### Phase 1: Detecting the Cycle
- Initialize `slow` and `fast` pointers (usually at head).
- `slow` moves 1 step: `slow = slow.next`
- `fast` moves 2 steps: `fast = fast.next.next`
- If `fast` reaches `null`, no cycle.
- If `slow == fast`, cycle exists.

### Phase 2: Finding Cycle Entrance
- After meeting in Phase 1, keep `fast` at meeting point.
- Reset `slow` to `head`.
- Move both `slow` and `fast` 1 step at a time.
- The node where they meet is the **entrance** to the cycle.

**Why Phase 2 works:**
Let $L$ be distance to entrance, $C$ be cycle length.
Meeting point is at $L + k$.
Distance covered by fast is $2 \times$ distance covered by slow.
Math shows distance remaining from meeting point to entrance is exactly $L$.

## Template/Pattern

```python
def find_cycle_start(head):
    slow = fast = head
    
    # Phase 1: Detect
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None  # No cycle
    
    # Phase 2: Find Entrance
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

## Array as Linked List

For array problems (like "Find the Duplicate Number"), treat:
- **Node:** Index `i`
- **Next Pointer:** Value `nums[i]`

```python
# Move 1 step
slow = nums[slow]

# Move 2 steps
fast = nums[nums[fast]]
```

## Problems Using This Technique

- [[solutions/arrays/find-the-duplicate-number]]
- [[solutions/linked-lists/linked-list-cycle]]

## Related Techniques

- [[techniques/two-pointers]] - General category
- [[techniques/fast-slow-pointers]] - Broad term for this pattern

## Tips and Pitfalls

- **Null Checks:** Always check `fast` and `fast.next` for null in linked lists.
- **Start Position:** Usually both start at head. If starting elsewhere (e.g., `-1`), math for Phase 2 might need adjustment.
- **Infinite Loop:** Ensure pointers actually move (e.g., `fast` moves faster than `slow`).
