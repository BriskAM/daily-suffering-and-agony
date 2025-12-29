# Linked List Cycle

**Source:** LeetCode #141  
**Difficulty:** Easy  
**Date Solved:** 2025-12-30

## Problem Statement

Given the head of a linked list, determine if the linked list has a cycle in it. A cycle exists if there is some node in the list that can be reached again by continuously following the `next` pointer.

## Alternative Approaches

This problem has two common solutions with different space complexity trade-offs.

---

### Approach 1: HashSet (Visited Tracking)

**Primary Technique:** [[methods/built-ins/set]]  
**Pattern:** Track visited nodes

Use a set to remember which nodes we've seen. If we encounter a node that's already in the set, there's a cycle.

#### Solution

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited = set()
        curr = head

        while curr:
            if curr in visited:
                return True
            visited.add(curr)
            curr = curr.next

        return False
```

#### Complexity Analysis

- **Time Complexity:** O(n) - visit each node once
- **Space Complexity:** O(n) - store up to n nodes in the set

#### Pros & Cons

✅ **Pros:**
- Simple and intuitive
- Easy to understand and implement
- Works for finding the cycle start with minor modification

❌ **Cons:**
- Uses O(n) extra space
- Slower in practice due to set operations

---

### Approach 2: Floyd's Cycle Detection (Tortoise and Hare)

**Primary Technique:** [[techniques/pointer-manipulation]]  
**Pattern:** Fast/slow two-pointer

Use two pointers moving at different speeds. If there's a cycle, the fast pointer will eventually catch up to the slow pointer.

#### Solution

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True
        
        return False
```

#### Complexity Analysis

- **Time Complexity:** O(n) - in worst case, fast pointer traverses the cycle
- **Space Complexity:** O(1) - only two pointers

#### Pros & Cons

✅ **Pros:**
- O(1) space complexity (optimal)
- Faster in practice (no hash operations)
- Classic algorithm worth knowing

❌ **Cons:**
- Less intuitive at first
- Requires understanding of why it works

---

## Trade-off Analysis

| Aspect | HashSet | Floyd's Algorithm |
|--------|---------|-------------------|
| **Space** | O(n) | O(1) ✅ |
| **Time** | O(n) | O(n) |
| **Simplicity** | More intuitive ✅ | Requires proof |
| **Interview** | Good for explaining | Shows advanced knowledge ✅ |
| **Practical** | Slower (hash ops) | Faster ✅ |

### When to Use Which?

**Use HashSet when:**
- Space is not a constraint
- You need to find the cycle start node (easier to modify)
- Clarity is more important than optimization

**Use Floyd's when:**
- Space is limited (embedded systems, large lists)
- You want optimal solution
- In interviews to demonstrate algorithm knowledge

---

## Key Insights

### Why Floyd's Algorithm Works

If there's a cycle:
1. Both pointers eventually enter the cycle
2. Once inside, the fast pointer is "chasing" the slow pointer
3. The gap closes by 1 each iteration (fast gains 2, slow gains 1)
4. They must eventually meet

### Edge Cases Handled

- Empty list (`head is None`)
- Single node with no cycle
- Single node pointing to itself
- Cycle at the end vs cycle in the middle

---

## Related Problems

- [[solutions/linked-lists/reverse-linked-list]] - Also uses pointer manipulation
- [[solutions/linked-lists/merge-two-sorted-lists]] - Different pointer pattern
- [[solutions/linked-lists/linked-list-cycle-ii]] - Find the cycle start (extension)

---

## Techniques Used

- [[techniques/pointer-manipulation]] (Floyd's algorithm)
- [[methods/built-ins/set]] (HashSet approach)
