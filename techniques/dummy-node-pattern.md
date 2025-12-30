# Dummy Node Pattern

## What is it?

The dummy node pattern involves creating a temporary "dummy" or "sentinel" node at the beginning of a linked list to simplify edge case handling. Instead of special-casing operations on the head, you treat all nodes uniformly and return `dummy.next` as the actual head at the end.

## When to Use

- Building a new linked list from scratch
- Merging multiple linked lists
- Removing nodes (especially when head might be removed)
- Partitioning or reordering lists
- Any operation where the head might change

**Key indicators:**
- You find yourself writing special cases for "if this is the head node..."
- You're building a result list node by node
- The head of the list might be removed or changed
- You want cleaner, more uniform code

## Template/Pattern

```python
def linkedListOperation(head):
    # Create dummy node
    dummy = ListNode(0)  # Value doesn't matter
    tail = dummy  # Pointer to track where to append
    
    # Process nodes...
    while condition:
        # Do work...
        tail.next = some_node
        tail = tail.next
    
    # Return actual head (skip dummy)
    return dummy.next
```

## Why It Works

Without dummy node:
```python
# Need special case for first node
result = None
tail = None

if some_condition:
    result = node1
    tail = result
else:
    result = node2
    tail = result

# Then continue with rest...
```

With dummy node:
```python
# No special case needed!
dummy = ListNode(0)
tail = dummy

if some_condition:
    tail.next = node1
else:
    tail.next = node2
tail = tail.next

return dummy.next
```

## Problems Using This Technique

- [[solutions/linked-lists/merge-two-sorted-lists]] - Classic dummy node usage
- [[solutions/linked-lists/remove-nth-from-end]] - Simplifies head removal
- [[solutions/linked-lists/add-two-numbers]] - Building new list with carry
- [[solutions/linked-lists/partition-list]] - Building two lists

## Related Techniques

- [[techniques/pointer-manipulation]] - Often used together
- [[techniques/two-pointers]] - Can be combined

## Tips & Pitfalls

### Tips
- The dummy node's value doesn't matter (often set to 0 or -1)
- Always return `dummy.next`, not `dummy`
- Use a `tail` pointer to track where to append next
- This pattern trades one extra node for much cleaner code
- Works for both creating new nodes and rearranging existing ones

### Common Pitfalls
- Forgetting to return `dummy.next` instead of `dummy`
- Not updating the tail pointer after appending (`tail = tail.next`)
- Creating a new dummy inside a loop (should be created once)
- Worrying about the "wasted" dummy node (it's worth it for code clarity!)

## Space Complexity Note

The dummy node uses O(1) extra space - just one additional node regardless of input size. This is negligible and worth the code simplification.
