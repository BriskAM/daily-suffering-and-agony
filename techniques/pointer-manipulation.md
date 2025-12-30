# Pointer Manipulation

## What is it?

Pointer manipulation is a fundamental technique for working with linked lists where you carefully manage multiple pointers to traverse, modify, or restructure the list. This typically involves maintaining 2-3 pointers that move through the list in coordinated ways.

## When to Use

- Reversing a linked list
- Detecting cycles (Floyd's algorithm)
- Finding the middle of a list
- Removing nodes in-place
- Reordering or partitioning a list
- Any time you need to modify links between nodes

**Key indicators:**
- Problem requires in-place modification of a linked list
- Need to track relationships between adjacent or nearby nodes
- Want O(1) space complexity (no extra data structures)

## Common Patterns

### Three-Pointer Reversal
Used to reverse a linked list iteratively:

```python
def reverseList(head):
    prev = None
    curr = head
    
    while curr:
        nxt = curr.next  # Save next node
        curr.next = prev  # Reverse the link
        prev = curr       # Move prev forward
        curr = nxt        # Move curr forward
    
    return prev  # New head
```

### Two-Pointer (Fast/Slow)
Used to find the middle or detect cycles:

```python
def findMiddle(head):
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow  # Middle node
```

### Floyd's Cycle Detection (Tortoise and Hare)
Used to detect cycles in O(1) space:

```python
def hasCycle(head):
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True  # Cycle detected
    
    return False  # No cycle
```

**Why it works:** If there's a cycle, the fast pointer will eventually "lap" the slow pointer inside the cycle. The gap closes by 1 each iteration since fast gains 2 positions while slow gains 1.


### Dummy Node + Tail Pointer
Used for building or merging lists:

```python
def mergeLists(list1, list2):
    dummy = ListNode(0)
    tail = dummy
    
    # Process nodes...
    # tail.next = selected_node
    # tail = tail.next
    
    return dummy.next
```

## Problems Using This Technique

- [[solutions/linked-lists/reverse-linked-list]] - Three-pointer reversal
- [[solutions/linked-lists/merge-two-sorted-lists]] - Tail pointer manipulation
- [[solutions/linked-lists/linked-list-cycle]] - Floyd's cycle detection
- [[solutions/linked-lists/reorder-list]] - Combines multiple patterns

## Related Techniques

- [[techniques/dummy-node-pattern]] - Often used together
- [[techniques/two-pointers]] - Similar concept applied to arrays

## Tips & Pitfalls

### Tips
- Always save `curr.next` before modifying `curr.next`, or you'll lose the rest of the list
- Draw diagrams! Pointer manipulation is highly visual
- Consider what happens at boundaries (empty list, single node, last node)
- Use meaningful variable names: `prev`, `curr`, `nxt` are clearer than `p1`, `p2`, `p3`

### Common Pitfalls
- Forgetting to move pointers forward (infinite loop)
- Losing reference to the rest of the list by modifying pointers too early
- Not handling None/null cases (empty list, reaching end)
- Off-by-one errors when using fast/slow pointers
