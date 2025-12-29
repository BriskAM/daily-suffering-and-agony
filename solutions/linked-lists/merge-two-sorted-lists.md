# Merge Two Sorted Lists

**Source:** LeetCode #21  
**Difficulty:** Easy  
**Date Solved:** 2025-12-29

## Problem Statement

Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

## Approach

**Primary Technique:** [[techniques/dummy-node-pattern]]  
**Secondary Techniques:** [[techniques/two-pointers]], [[techniques/pointer-manipulation]]

The dummy node pattern simplifies edge cases:
1. Create a dummy node to avoid special-casing the head
2. Use a `tail` pointer to track where to append next
3. Compare values from both lists and append the smaller one
4. Move the pointer of the list we took from
5. After the loop, append any remaining nodes
6. Return `dummy.next` (the actual head)

This avoids having to check if we're at the head of the result list.

## Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        tail = dummy
        
        while list1 and list2:
            if list1.val <= list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                tail.next = list2
                list2 = list2.next
            tail = tail.next
        
        # Append remaining nodes (at most one list has remaining nodes)
        tail.next = list1 if list1 else list2
        
        return dummy.next
```

## Complexity Analysis

- **Time Complexity:** O(n + m) - where n and m are lengths of the two lists
- **Space Complexity:** O(1) - only using pointers, not creating new nodes

## Key Insights

- Dummy node eliminates the need to special-case the first node
- We don't create new nodes, just rearrange pointers (space efficient)
- The condition `list1 and list2` handles when either list is exhausted
- The final `tail.next = list1 if list1 else list2` appends all remaining nodes in one step
- Using `<=` instead of `<` maintains stability (preserves relative order)

## Related Problems

- [[solutions/linked-lists/reverse-linked-list]] - Also uses pointer manipulation
- [[solutions/linked-lists/merge-k-sorted-lists]] - Extension to k lists

## Techniques Used

- [[techniques/dummy-node-pattern]]
- [[techniques/two-pointers]]
- [[techniques/pointer-manipulation]]
