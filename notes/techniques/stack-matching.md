---
tags:
  - technique/stack
  - pattern/matching
  - data-structure/stack
---

# Stack Matching Pattern

## What is it?

Using a stack to validate matching pairs or nested structures. The stack's LIFO (Last In, First Out) property naturally handles nested/recursive relationships.

## When to Use

- Validating balanced parentheses/brackets
- Matching opening and closing tags (HTML/XML)
- Expression evaluation (postfix, infix)
- Backtracking patterns
- Nested structure validation
- "Most recent unmatched" problems

## Template/Pattern

```python
def is_valid_matching(s):
    stack = []
    pairs = {'(': ')', '{': '}', '[': ']'}  # opening -> closing
    
    for char in s:
        if char in pairs:  # Opening element
            stack.append(char)
        else:  # Closing element
            if not stack:  # No matching opening
                return False
            if pairs[stack.pop()] != char:  # Mismatch
                return False
    
    return len(stack) == 0  # All opened elements closed
```

## Common Variations

### 1. Bracket Matching (Classic)

```python
def isValid(s):
    stack = []
    closing = {')': '(', '}': '{', ']': '['}
    
    for ch in s:
        if ch in closing:
            if not stack or stack.pop() != closing[ch]:
                return False
        else:
            stack.append(ch)
    
    return not stack
```

### 2. HTML/XML Tag Matching

```python
def isValidHTML(html):
    stack = []
    i = 0
    
    while i < len(html):
        if html[i] == '<':
            # Find tag end
            j = html.index('>', i)
            tag = html[i+1:j]
            
            if tag.startswith('/'):  # Closing tag
                if not stack or stack.pop() != tag[1:]:
                    return False
            else:  # Opening tag
                stack.append(tag)
            
            i = j + 1
        else:
            i += 1
    
    return not stack
```

### 3. Remove Outermost Parentheses

```python
def removeOuterParentheses(s):
    result = []
    depth = 0
    
    for ch in s:
        if ch == '(':
            if depth > 0:  # Not outermost
                result.append(ch)
            depth += 1
        else:  # ')'
            depth -= 1
            if depth > 0:  # Not outermost
                result.append(ch)
    
    return ''.join(result)
```

### 4. Longest Valid Parentheses

```python
def longestValidParentheses(s):
    stack = [-1]  # Base for valid substring
    max_len = 0
    
    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
        else:
            stack.pop()
            if not stack:
                stack.append(i)
            else:
                max_len = max(max_len, i - stack[-1])
    
    return max_len
```

## Stack Operations

| Operation | Python List | Complexity |
|-----------|-------------|------------|
| Push | `stack.append(x)` | O(1) |
| Pop | `stack.pop()` | O(1) |
| Peek/Top | `stack[-1]` | O(1) |
| Is Empty | `not stack` or `len(stack) == 0` | O(1) |

## Key Advantages

| Advantage | Explanation |
|-----------|-------------|
| Natural nesting | LIFO matches nested structure |
| O(1) operations | All stack ops are constant time |
| Simple implementation | Just use a list in Python |
| Space efficient | Only stores unmatched elements |

## Stack vs Other Approaches

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Stack | O(n) | O(n) | Optimal for matching |
| String replacement | O(n²) | O(n) | Repeated string ops |
| Counter | N/A | N/A | Can't handle order/nesting |
| Two pointers | N/A | N/A | Can't handle nesting |

## Common Pitfalls

1. **Empty stack check:** Always check `if not stack` before `stack.pop()`
2. **Final validation:** Check `len(stack) == 0` at end
3. **Indexing errors:** Use `stack[-1]` for peek, not `stack[len(stack)-1]`
4. **Mutable elements:** If storing lists/dicts, be careful with references

## Problems Using This Technique

- [[solutions/stack-queue/valid-parentheses]] - Classic bracket matching

## Related Techniques

- [[techniques/monotonic-stack]] - Stack with ordering constraint
- [[techniques/recursion]] - Alternative for nested structures

## Tips and Best Practices

- **Use list as stack:** Python lists are optimized for stack operations
- **Early return:** Return `False` immediately on mismatch
- **Dictionary for pairs:** Clean mapping between opening/closing
- **Track depth:** Some problems need depth counter alongside stack
- **Base case:** For interval problems, initialize stack with sentinel value
