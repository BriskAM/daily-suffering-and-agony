# Reverse Linked List

**Source:** LeetCode #206  
**Difficulty:** Easy  
**Date Solved:** 2025-12-29

## Problem Statement

Given the head of a singly linked list, reverse the list and return the reversed list.

## Approach

**Primary Technique:** [[techniques/pointer-manipulation]]  
**Pattern:** Three-pointer iterative reversal

The key insight is to use three pointers to reverse the direction of links as we traverse:
- `prev`: tracks the previous node (starts as None)
- `curr`: tracks the current node being processed
- `nxt`: temporarily stores the next node before we break the link

At each step:
1. Save the next node (`nxt = curr.next`)
2. Reverse the current link (`curr.next = prev`)
3. Move both pointers forward (`prev = curr`, `curr = nxt`)

When `curr` becomes None, `prev` points to the new head.

## Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        curr = head

        while curr:
            nxt = curr.next
            curr.next = prev
            prev = curr
            curr = nxt

        return prev
```

## Complexity Analysis

- **Time Complexity:** O(n) - single pass through the list
- **Space Complexity:** O(1) - only using three pointers

## Key Insights

- The three-pointer pattern is essential for in-place linked list manipulation
- We must save `curr.next` before modifying `curr.next`, otherwise we lose the rest of the list
- `prev` starts as None because the original head becomes the tail (which points to None)
- This is the iterative approach; there's also a recursive solution

## Related Problems

- [[solutions/linked-lists/merge-two-sorted-lists]] - Also uses pointer manipulation
- [[solutions/linked-lists/palindrome-linked-list]] - Uses reversal as a subroutine

## Techniques Used

- [[techniques/pointer-manipulation]]
