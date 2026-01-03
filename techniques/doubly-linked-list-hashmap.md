# Doubly Linked List + HashMap Pattern

## What is it?

A hybrid data structure combining a **doubly linked list** with a **hash map** to achieve O(1) time complexity for:
- Lookup by key
- Insertion at any position
- Removal from any position
- Access to first/last elements

This pattern is the foundation for LRU Cache, LFU Cache, and similar ordered-access data structures.

## When to Use

- Design problems requiring O(1) for both lookup AND ordered operations
- Cache implementations with eviction policies
- Maintaining insertion/access order with fast key-based retrieval
- Any problem asking for "O(1) get, O(1) add, O(1) remove with ordering"

**Key indicators:**
- Problem mentions "O(1)" for multiple operations
- Need to track "least/most recently used"
- Need both key-based access AND positional operations
- "Design a data structure that supports..."

## Why Both Structures?

| Operation | HashMap Only | DLL Only | Combined |
|-----------|--------------|----------|----------|
| Lookup by key | O(1) ✓ | O(n) ✗ | O(1) ✓ |
| Remove by key | O(1) ✓ | O(n) ✗ | O(1) ✓ |
| Insert at position | N/A | O(1) ✓ | O(1) ✓ |
| Remove from position | N/A | O(1) ✓ | O(1) ✓ |
| Access first/last | N/A | O(1) ✓ | O(1) ✓ |

Neither alone gives O(1) for everything—together they do!

## Template/Pattern

```python
class Node:
    def __init__(self, key=0, value=0):
        self.key = key      # Store key for HashMap deletion
        self.value = value
        self.prev = None
        self.next = None


class DLLWithHashMap:
    def __init__(self):
        self.cache = {}         # key -> Node
        self.head = Node()      # Dummy head
        self.tail = Node()      # Dummy tail
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def _remove(self, node: Node):
        """O(1) removal from any position."""
        node.prev.next = node.next
        node.next.prev = node.prev
    
    def _add_to_front(self, node: Node):
        """O(1) insertion at front."""
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def _add_to_back(self, node: Node):
        """O(1) insertion at back."""
        node.next = self.tail
        node.prev = self.tail.prev
        self.tail.prev.next = node
        self.tail.prev = node
    
    def get(self, key):
        if key not in self.cache:
            return None
        return self.cache[key]
    
    def remove_last(self):
        """O(1) removal of back element."""
        if self.tail.prev == self.head:
            return None
        node = self.tail.prev
        self._remove(node)
        del self.cache[node.key]
        return node
```

## Critical Implementation Details

### 1. Node Stores Key
```python
class Node:
    def __init__(self, key, value):
        self.key = key  # ← ESSENTIAL!
```
Without the key, you can't delete from HashMap during eviction:
```python
lru = self.tail.prev
self._remove(lru)
del self.cache[lru.key]  # Need lru.key!
```

### 2. Sentinel Nodes Eliminate Edge Cases
```python
# Without sentinels - many edge cases:
if self.head is None:  # Empty list
    ...
if node == self.head:  # Removing head
    ...
if node == self.tail:  # Removing tail
    ...

# With sentinels - uniform code:
def _remove(self, node):
    node.prev.next = node.next  # Always valid
    node.next.prev = node.prev  # Always valid
```

### 3. Pointer Update Order Matters
```python
def _add_to_front(self, node):
    # Set new node's pointers FIRST
    node.prev = self.head
    node.next = self.head.next
    # THEN update existing pointers
    self.head.next.prev = node  # Before changing head.next!
    self.head.next = node
```

## Problems Using This Technique

- [[solutions/linked-lists/lru-cache]] - Classic application (LeetCode #146)
- [[solutions/linked-lists/lfu-cache]] - With frequency tracking (LeetCode #460)
- [[solutions/linked-lists/all-oone-data-structure]] - Inc/Dec operations (LeetCode #432)

## Related Techniques

- [[techniques/dummy-node-pattern]] - Sentinel nodes are a form of this
- [[techniques/pointer-manipulation]] - Core linked list operations

## Tips & Pitfalls

### Tips
- Always test with capacity=1 (evict on every new insert)
- Draw the pointers on paper for tricky operations
- The HashMap value is the Node, not just the value
- Sentinel nodes are worth the tiny memory overhead

### Common Pitfalls
- Forgetting to store key in Node (can't delete from HashMap)
- Wrong order of pointer updates (corrupts the list)
- Not removing node before re-adding (creates duplicates/cycles)
- Off-by-one in size checks (`len(cache) == capacity` vs `>`)

## Python Alternative: collections.OrderedDict

Python's `OrderedDict` internally uses this exact pattern:

```python
from collections import OrderedDict

# move_to_end() = remove + add_to_front/back
# popitem(last=False) = remove_first
# popitem(last=True) = remove_last
```

Use `OrderedDict` in production; implement manually in interviews.
