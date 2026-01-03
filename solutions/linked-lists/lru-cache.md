# LRU Cache

**Source:** LeetCode #146  
**Difficulty:** Medium  
**Date Solved:** 2026-01-03

## Problem Statement

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the `LRUCache` class:
- `LRUCache(int capacity)` Initialize the LRU cache with positive size capacity.
- `int get(int key)` Return the value of the key if it exists, otherwise return -1.
- `void put(int key, int value)` Update or insert value. Evict the LRU key if capacity exceeded.

Both `get` and `put` must run in **O(1)** average time complexity.

## Approach

**Primary Technique:** [[techniques/doubly-linked-list-hashmap]]  
**Secondary Techniques:** [[techniques/dummy-node-pattern]], [[techniques/pointer-manipulation]]

The O(1) requirement demands two complementary data structures:

1. **HashMap** (`self.cache`): O(1) key → node lookup
2. **Doubly Linked List**: O(1) removal and insertion at any position

**Why Doubly Linked List?**
- Singly linked list removal is O(n) (need to find previous node)
- Doubly linked list removal is O(1) (have direct access to prev and next)

**Sentinel Nodes (Head/Tail):**
- Dummy head and tail eliminate null checks
- Most recently used → front (after head)
- Least recently used → back (before tail)

**Operations:**
- `get(key)`: Find node, move to front, return value
- `put(key, value)`: Add/update node at front, evict from back if over capacity

## Solution

```python
class Node:
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}  # key -> Node
        self.head = Node()  # Dummy head (MRU side)
        self.tail = Node()  # Dummy tail (LRU side)
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node):
        """Remove node from its current position in the list."""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node

    def _add_to_front(self, node: Node):
        """Add node right after the dummy head (MRU position)."""
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1

        node = self.cache[key]
        self._remove(node)
        self._add_to_front(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.value = value
            self._remove(node)
            self._add_to_front(node)
        else:
            if len(self.cache) == self.capacity:
                # Evict LRU (node before tail)
                lru = self.tail.prev
                self._remove(lru)
                del self.cache[lru.key]

            new_node = Node(key, value)
            self.cache[key] = new_node
            self._add_to_front(new_node)
```

## Complexity Analysis

- **Time Complexity:** O(1) for both `get` and `put`
  - HashMap lookup: O(1)
  - Doubly linked list remove: O(1)
  - Doubly linked list add: O(1)
- **Space Complexity:** O(capacity) - storing up to capacity nodes + HashMap entries

## Key Insights

- **Node stores key**: Essential for HashMap deletion during eviction (need `lru.key` to call `del self.cache[lru.key]`)
- **Sentinel nodes simplify code**: No null checks for head.prev or tail.next
- **Move-to-front on access**: Both `get` and `put` move the node to front
- **Evict from back**: LRU item is always `tail.prev`
- **Remove before add**: When updating existing key, remove from current position first
- **Check capacity before insert**: Evict LRU before adding new node, not after

## Visual Representation

```
  head ←→ [MRU] ←→ [node] ←→ [node] ←→ [LRU] ←→ tail
    ↑                                      ↑
  dummy                                  dummy
  
get/put moves node to front:
  head ←→ [accessed] ←→ [MRU] ←→ [node] ←→ [LRU] ←→ tail
```

## Related Problems

- [[solutions/linked-lists/lfu-cache]] - Frequency-based eviction (LeetCode #460)
- [[solutions/linked-lists/design-linked-list]] - Basic doubly linked list ops (LeetCode #707)
- [[solutions/linked-lists/all-oone-data-structure]] - Similar dual structure (LeetCode #432)

## Alternative Approaches

### OrderedDict (Python shortcut)

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)
        return self.cache[key]

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
```

**Trade-off**: OrderedDict is cleaner but hides the underlying DLL+HashMap pattern. The manual implementation is preferred for interviews to demonstrate understanding.

## Techniques Used

- [[techniques/doubly-linked-list-hashmap]]
- [[techniques/dummy-node-pattern]]
- [[techniques/pointer-manipulation]]
