---
tags:
  - difficulty/easy
  - technique/two-pointers
  - technique/string-manipulation
  - pattern/palindrome
  - source/leetcode
  - status/solved
---

# Valid Palindrome

**Source:** LeetCode #125  
**Difficulty:** Easy  
**Date Solved:** 2024-12-28

## Problem Statement

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

## Approach

**Primary Technique:** [[notes/techniques/two-pointers]]  
**Secondary Techniques:** [[methods/built-ins/str-methods]]

### Strategy

1. **Clean the string**: Remove non-alphanumeric characters and convert to lowercase
2. **Compare from both ends**: Use index `i` from start and `-1-i` from end

### Why This Works

A palindrome reads the same forwards and backwards. By cleaning first, we only need to compare positions symmetrically from both ends.

## Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = ''.join(c for c in s if c.isalnum()).lower()
        n = len(s)
        for i in range(int(n/2)):
            if s[i] != s[-1-i]:
                return False
        return True
```

### Pythonic One-Liner

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        cleaned = ''.join(c.lower() for c in s if c.isalnum())
        return cleaned == cleaned[::-1]
```

### Alternative: O(1) Space with Two Pointers

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1
        
        while left < right:
            # Skip non-alphanumeric from left
            while left < right and not s[left].isalnum():
                left += 1
            # Skip non-alphanumeric from right
            while left < right and not s[right].isalnum():
                right -= 1
            
            if s[left].lower() != s[right].lower():
                return False
            
            left += 1
            right -= 1
        
        return True
```

## Complexity Analysis

| Approach | Time | Space |
|----------|------|-------|
| Clean + Compare | O(n) | O(n) - new string |
| Reverse Compare | O(n) | O(n) - reversed string |
| **True Two Pointers** | O(n) | **O(1)** |

## Key Insights

1. **`isalnum()`** checks if character is alphanumeric (letter or digit)
2. **Negative indexing**: `s[-1-i]` accesses from end (-1 is last, -2 is second last, etc.)
3. **Range optimization**: Only need to check `n/2` characters
4. **Two pointer variant** avoids creating new string → O(1) space

## Edge Cases

- Empty string → `True` (vacuously a palindrome)
- Single character → `True`
- All non-alphanumeric: `".,,"` → `True` (cleaned = "")
- Mixed case: `"Aa"` → `True`

## Related Problems

- [[solutions/two-pointers/palindrome-linked-list]] - Same concept on linked list
- [[solutions/strings/longest-palindromic-substring]] - Find palindrome within string
- [[solutions/two-pointers/two-sum-ii]] - Another two pointer pattern

## Techniques Used

- [[notes/techniques/two-pointers]] - Compare from both ends
- [[methods/built-ins/str-methods]] - `isalnum()`, `lower()`, `join()`

## Review Log

| Date | Confidence | Notes |
|------|------------|-------|
| 2024-12-28 | 5/5 | Clean solution, understood both approaches |
