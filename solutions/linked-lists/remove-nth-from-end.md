# Remove Nth Node From End of List

**Source:** LeetCode #19  
**Difficulty:** Medium  
**Date Solved:** 2025-12-30

## Problem Statement

Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Example:**
- Input: `head = [1,2,3,4,5], n = 2`
- Output: `[1,2,3,5]`

## Approach

**Primary Techniques:** [[techniques/dummy-node-pattern]], [[techniques/pointer-manipulation]]  
**Pattern:** Two-pointer with n-gap + dummy node

The key insight is to use **two pointers with a fixed gap of n nodes** between them. When the fast pointer reaches the end, the slow pointer will be exactly at the node before the one to remove.

### Why Dummy Node?

The dummy node is crucial here because the node to remove might be the head itself (when `n` equals the list length). Without a dummy node, we'd need special handling for this case.

### The Two-Pointer Technique

1. **Create dummy node** pointing to head
2. **Position both pointers** at dummy
3. **Move fast pointer n steps ahead** to create the gap
4. **Move both pointers together** until fast reaches the end
5. **Remove the node** by skipping it (`slow.next = slow.next.next`)
6. **Return dummy.next** (the actual head)

## Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        slow = fast = dummy
        
        # Move fast pointer n steps ahead
        for i in range(n):
            fast = fast.next
        
        # Move both pointers until fast reaches the end
        while fast.next:
            slow = slow.next
            fast = fast.next
        
        # Remove the nth node from end
        slow.next = slow.next.next
        
        return dummy.next
```

## Complexity Analysis

- **Time Complexity:** O(L) where L is the length of the list - single pass
- **Space Complexity:** O(1) - only using two pointers and one dummy node

## Key Insights

### The n-Gap Technique
By maintaining exactly n nodes between slow and fast:
- When fast is at the last node, slow is at the node before the target
- This works because: `(L - n - 1)` positions from start = node before nth from end

### Why Start at Dummy?
```
List: 1 → 2 → 3 → 4 → 5, n = 2
      ↑
    dummy

After moving fast n steps:
dummy → 1 → 2 → 3 → 4 → 5
↑           ↑
slow        fast

After moving both to end:
dummy → 1 → 2 → 3 → 4 → 5
        ↑           ↑
        slow        fast

Now slow.next is the node to remove!
```

### Edge Cases Handled
- **Removing the head:** Dummy node makes this seamless
- **Single node list:** Works correctly
- **n equals list length:** Removes the head without special case

### One-Pass Solution
Unlike the naive approach (count length, then remove), this solves it in a single pass by cleverly using the two-pointer gap.

## Related Problems

- [[solutions/linked-lists/reverse-linked-list]] - Also uses pointer manipulation
- [[solutions/linked-lists/merge-two-sorted-lists]] - Uses dummy node pattern
- [[solutions/linked-lists/linked-list-cycle]] - Fast/slow pointer pattern
- [[solutions/linked-lists/reorder-list]] - Combines multiple techniques

## Techniques Used

- [[techniques/dummy-node-pattern]]
- [[techniques/pointer-manipulation]]
