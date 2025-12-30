# Reorder List

**Source:** LeetCode #143  
**Difficulty:** Medium  
**Date Solved:** 2025-12-30

## Problem Statement

Given a singly linked list `L: L0 → L1 → ... → Ln-1 → Ln`, reorder it to: `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`

You must do this **in-place** without altering the node values.

**Example:**
- Input: `1 → 2 → 3 → 4 → 5`
- Output: `1 → 5 → 2 → 4 → 3`

## Approach

**Primary Techniques:** [[techniques/pointer-manipulation]]  
**Pattern:** Multi-step linked list manipulation

This problem is a **masterclass in combining multiple linked list techniques**! It requires three distinct steps:

1. **Find the middle** (fast/slow pointers)
2. **Reverse the second half** (three-pointer reversal)
3. **Merge the two halves** (interleaving merge)

Each step uses techniques we've seen before, making this a great problem for practicing composition of patterns.

### Step-by-Step Breakdown

#### Step 1: Find the Middle
Use fast/slow pointers to find the middle of the list:
```python
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
# slow is now at the middle
```

#### Step 2: Split and Reverse Second Half
Split the list at the middle and reverse the second half:
```python
prev = None
curr = slow.next
slow.next = None  # Split the list

while curr:
    nxt = curr.next
    curr.next = prev
    prev = curr
    curr = nxt
# prev is now the head of reversed second half
```

#### Step 3: Merge Two Halves
Interleave nodes from both halves:
```python
first = head
second = prev

while second:
    t1 = first.next
    t2 = second.next
    
    first.next = second
    second.next = t1
    
    first = t1
    second = t2
```

## Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        # Step 1: Find middle
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        # Step 2: Reverse second half
        prev = None
        curr = slow.next
        slow.next = None  # Split the list

        while curr:
            nxt = curr.next
            curr.next = prev
            prev = curr
            curr = nxt

        # Step 3: Merge two halves
        first = head
        second = prev

        while second:
            t1 = first.next
            t2 = second.next

            first.next = second
            second.next = t1

            first = t1
            second = t2
```

## Complexity Analysis

- **Time Complexity:** O(n) - three passes through the list (find middle, reverse, merge)
- **Space Complexity:** O(1) - only using pointers, in-place modification

## Key Insights

### Why This Problem is Important
- **Combines multiple techniques:** Tests your ability to compose solutions
- **In-place requirement:** Forces O(1) space solution
- **No new nodes created:** Pure pointer manipulation

### Critical Details
1. **Must split the list** (`slow.next = None`) before reversing, otherwise you create a cycle
2. **Second half might be shorter:** For odd-length lists, first half has one more node
3. **Loop condition:** `while second` (not `while first and second`) because second is always shorter or equal
4. **Save next pointers:** Must save both `t1` and `t2` before modifying links

### Visual Example
```
Original:    1 → 2 → 3 → 4 → 5

After split:
First half:  1 → 2 → 3
Second half: 4 → 5

After reverse second:
First half:  1 → 2 → 3
Second half: 5 → 4

After merge:
Result:      1 → 5 → 2 → 4 → 3
```

## Related Problems

- [[solutions/linked-lists/reverse-linked-list]] - Used in Step 2
- [[solutions/linked-lists/linked-list-cycle]] - Fast/slow pointer pattern (Step 1)
- [[solutions/linked-lists/merge-two-sorted-lists]] - Similar merge concept
- [[solutions/linked-lists/palindrome-linked-list]] - Also uses find middle + reverse

## Techniques Used

- [[techniques/pointer-manipulation]] - All three steps use this
