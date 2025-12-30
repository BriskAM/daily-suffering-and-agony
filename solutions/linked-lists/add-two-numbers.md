# Add Two Numbers

**Source:** LeetCode #2  
**Difficulty:** Medium  
**Date Solved:** 2025-12-30

## Problem Statement

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

**Example:**
- Input: `l1 = [2,4,3]`, `l2 = [5,6,4]`
- Output: `[7,0,8]`
- Explanation: 342 + 465 = 807

## Approach

**Primary Techniques:** [[techniques/dummy-node-pattern]], [[techniques/pointer-manipulation]]  
**Pattern:** Build new list with dummy node + carry arithmetic

This problem simulates elementary school addition: add digits from right to left, carrying over when the sum exceeds 9. Since the digits are already in reverse order, we can process them directly.

### Key Insights

1. **Dummy node simplifies list building** - no special case for the first digit
2. **Process both lists simultaneously** - handle different lengths gracefully
3. **Carry must be tracked** - can persist even after both lists are exhausted
4. **Loop condition:** `while l1 or l2 or carry` - continue until all three are done

### Step-by-Step

1. Create dummy node and current pointer
2. Initialize carry to 0
3. While there are digits to process OR carry exists:
   - Get values from both lists (0 if list is exhausted)
   - Calculate total = val1 + val2 + carry
   - Extract new carry (total // 10) and digit (total % 10)
   - Create new node with digit
   - Move pointers forward
4. Return dummy.next

## Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        current = dummy
        carry = 0

        while l1 or l2 or carry:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0

            total = val1 + val2 + carry
            carry = total // 10
            digit = total % 10

            current.next = ListNode(digit)
            current = current.next

            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

        return dummy.next
```

## Complexity Analysis

- **Time Complexity:** O(max(m, n)) - where m and n are lengths of the two lists
- **Space Complexity:** O(max(m, n)) - for the result list (not counting output)

## Key Insights

### Why the Loop Condition Works

```python
while l1 or l2 or carry:
```

This handles all cases elegantly:
- **Different length lists:** Continue processing the longer list
- **Final carry:** If last addition produces carry (e.g., 5+5=10), we need one more node
- **Example:** 99 + 1 = 100 requires the carry to create the final "1"

### Handling Exhausted Lists

```python
val1 = l1.val if l1 else 0
val2 = l2.val if l2 else 0
```

Treat exhausted lists as contributing 0, allowing uniform processing.

### Visual Example

```
  342:  2 → 4 → 3
+ 465:  5 → 6 → 4
-----  -----------
  807:  7 → 0 → 8

Step 1: 2+5=7, carry=0 → create node(7)
Step 2: 4+6=10, carry=1 → create node(0)
Step 3: 3+4+1=8, carry=0 → create node(8)
Done!
```

### Edge Cases Handled

- **Different length lists:** `[9,9] + [1]` = `[0,0,1]`
- **Carry at the end:** `[5] + [5]` = `[0,1]`
- **Single digit:** `[0] + [0]` = `[0]`

## Related Problems

- [[solutions/linked-lists/merge-two-sorted-lists]] - Also builds new list with dummy node
- [[solutions/linked-lists/remove-nth-from-end]] - Uses dummy node pattern

## Techniques Used

- [[techniques/dummy-node-pattern]]
- [[techniques/pointer-manipulation]]
