# Fast-Slow Pointers (Tortoise and Hare)

## What is it?

A technique using two pointers moving at different speeds to detect cycles, find middle elements, or identify specific positions in sequences. The "slow" pointer moves 1 step at a time; the "fast" pointer moves 2 steps.

## When to Use

- Detecting cycles in linked lists or arrays
- Finding the middle of a linked list
- Finding the start of a cycle (Floyd's algorithm)
- Detecting if a sequence eventually repeats

**Key indicators:**
- "Cycle detection" or "find duplicate"
- Links or indices that point to each other
- Need O(1) space for cycle problems
- Find middle without knowing length

## Template/Pattern

### Cycle Detection
```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### Find Cycle Start (Floyd's Algorithm)
```python
def find_cycle_start(head):
    # Phase 1: Detect cycle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    else:
        return None  # No cycle
    
    # Phase 2: Find entrance
    slow2 = head
    while slow != slow2:
        slow = slow.next
        slow2 = slow2.next
    return slow
```

### Find Middle of List
```python
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow  # Middle (or second middle if even length)
```

## Why It Works

### Cycle Detection
- If there's a cycle, fast will eventually "lap" slow inside the cycle
- If no cycle, fast reaches the end first

### Finding Cycle Start
Mathematical proof: Distance from head to cycle start = distance from meeting point to cycle start (going around)

## Problems Using This Technique

- [[solutions/linked-lists/linked-list-cycle]] - Detect if cycle exists
- [[solutions/arrays/find-the-duplicate-number]] - Array as implicit linked list
- [[solutions/linked-lists/reorder-list]] - Find middle to split list

## Related Techniques

- [[notes/techniques/floyds-cycle-detection]] - Specific application for cycle start
- [[techniques/pointer-manipulation]] - General pointer techniques

## Tips & Pitfalls

### Tips
- Always check `fast and fast.next` before advancing
- For "find middle", slow ends at middle when fast reaches end
- Can be applied to arrays by treating `arr[i]` as "next pointer"

### Pitfalls
- Null check order matters: `fast and fast.next`, not `fast.next and fast`
- Off-by-one for middle: even-length lists have two middles
- Phase 2 of Floyd's requires resetting ONE pointer to head, not both
