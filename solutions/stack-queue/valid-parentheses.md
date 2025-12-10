---
tags:
  - difficulty/easy
  - technique/stack
  - pattern/matching
  - company/google
  - company/amazon
  - company/microsoft
  - status/solved
---

# Valid Parentheses

**Source:** LeetCode #20  
**Difficulty:** Easy  
**Date Solved:** 2025-12-10

## Problem Statement

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets
2. Open brackets must be closed in the correct order
3. Every close bracket has a corresponding open bracket of the same type

**Example:**
```
Input: s = "()[]{}"
Output: true

Input: s = "([)]"
Output: false

Input: s = "{[]}"
Output: true
```

## Approach

**Primary Technique:** [[notes/techniques/stack-matching]]

Use a **stack** to track opening brackets. When encountering a closing bracket, verify it matches the most recent opening bracket (top of stack).

**Key Insight:** Stack's LIFO (Last In, First Out) property perfectly matches the nesting structure of valid parentheses.

## Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = list()
        corr = {
            '(': ')',
            '{': '}',
            '[': ']'
        }
        
        for ch in s:
            if ch in corr.keys():  # Opening bracket
                stack.append(ch)
            else:  # Closing bracket
                if not stack:  # No matching opening
                    return False
                
                top = stack.pop()
                if corr[top] != ch:  # Mismatch
                    return False
        
        return len(stack) == 0  # All brackets closed
```

## Complexity Analysis

- **Time Complexity:** O(n) - Single pass through string
- **Space Complexity:** O(n) - Stack can grow to size of input in worst case (e.g., all opening brackets)

## How It Works

**Example:** `s = "{[()]}"`

```
Process: {
  Stack: ['{']

Process: [
  Stack: ['{', '[']

Process: (
  Stack: ['{', '[', '(']

Process: )
  Pop '(' → corr['('] = ')' ✓ match
  Stack: ['{', '[']

Process: ]
  Pop '[' → corr['['] = ']' ✓ match
  Stack: ['{']

Process: }
  Pop '{' → corr['{'] = '}' ✓ match
  Stack: []

Final: Stack empty → Valid ✓
```

**Example:** `s = "([)]"`

```
Process: (
  Stack: ['(']

Process: [
  Stack: ['(', '[']

Process: )
  Pop '[' → corr['['] = ']' ✗ mismatch (expected ')')
  Return False
```

## Edge Cases Handled

| Case | Input | Output | Reason |
|------|-------|--------|--------|
| Empty string | `""` | `true` | No brackets to match |
| Only opening | `"((("` | `false` | Stack not empty at end |
| Only closing | `")))"` | `false` | Stack empty when popping |
| Mismatched type | `"([)]"` | `false` | Wrong bracket type |
| Valid nested | `"{[()]}"` | `true` | Proper nesting |
| Interleaved | `"()[]{}"` | `true` | Sequential pairs |

## Alternative Approaches

### Using Dictionary with Reverse Mapping

```python
def isValid(s):
    stack = []
    closing = {')': '(', '}': '{', ']': '['}
    
    for ch in s:
        if ch in closing:  # Closing bracket
            if not stack or stack.pop() != closing[ch]:
                return False
        else:  # Opening bracket
            stack.append(ch)
    
    return not stack
```

**Difference:** Maps closing → opening instead of opening → closing. Slightly cleaner logic.

### String Replacement (Inefficient)

```python
def isValid(s):
    while '()' in s or '[]' in s or '{}' in s:
        s = s.replace('()', '').replace('[]', '').replace('{}', '')
    return s == ''
```

**Trade-off:** O(n²) time due to repeated string replacements. Simple but inefficient.

## Key Insights

- **Stack for nesting:** LIFO matches bracket nesting structure
- **Opening vs closing:** Only push opening brackets, validate on closing
- **Empty stack check:** Before popping, ensure stack is not empty
- **Final check:** Stack must be empty (all brackets closed)
- **Dictionary mapping:** Clean way to associate bracket pairs

## Common Mistakes

1. **Forgetting empty stack check:** `stack.pop()` raises error on empty stack
2. **Not checking final stack:** `"((("` would pass without `len(stack) == 0`
3. **Wrong mapping direction:** Map opening → closing or closing → opening, not both
4. **Using set instead of dict:** Need pairing information, not just membership

## Related Problems

- [[solutions/strings/encode-decode-strings]] - String parsing pattern

## Techniques Used

- [[notes/techniques/stack-matching]]
- [[methods/built-ins/dict]]
- [[methods/built-ins/list-as-stack]]
