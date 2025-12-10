---
tags:
  - method/built-ins
  - python
  - string-manipulation
---

# String Slicing

**Module:** Built-in  
**Syntax:** `string[start:stop:step]`

## What is it?

Extract a substring using slice notation. Python's most powerful string operation for accessing parts of strings.

## When to Use

- Extracting substrings by index
- Parsing fixed or variable-length segments
- Reversing strings
- Skipping characters (using step)
- String manipulation without loops

## Syntax and Examples

```python
s = "hello world"

# Basic slicing
s[0:5]      # "hello" - from index 0 to 5 (exclusive)
s[6:]       # "world" - from index 6 to end
s[:5]       # "hello" - from start to index 5
s[:]        # "hello world" - full copy

# Negative indexing
s[-5:]      # "world" - last 5 characters
s[:-6]      # "hello" - all except last 6
s[-5:-1]    # "worl" - slice from end

# Step parameter
s[::2]      # "hlowrd" - every 2nd character
s[::-1]     # "dlrow olleh" - reverse string
s[::3]      # "hlwl" - every 3rd character

# Empty results
s[10:5]     # "" - invalid range returns empty
s[100:]     # "" - out of bounds returns empty (no error)
```

## Common Operations

| Operation | Syntax | Example |
|-----------|--------|---------|
| First n chars | `s[:n]` | `"hello"[:3]` → `"hel"` |
| Last n chars | `s[-n:]` | `"hello"[-2:]` → `"lo"` |
| Remove first n | `s[n:]` | `"hello"[2:]` → `"llo"` |
| Remove last n | `s[:-n]` | `"hello"[:-2]` → `"hel"` |
| Reverse | `s[::-1]` | `"hello"[::-1]` → `"olleh"` |
| Middle substring | `s[i:j]` | `"hello"[1:4]` → `"ell"` |

## DSA Patterns

```python
# Parse length-prefixed data
def parse_segment(s, i):
    j = s.index('#', i)
    length = int(s[i:j])
    data = s[j+1 : j+1+length]
    next_i = j + 1 + length
    return data, next_i

# Check palindrome
def is_palindrome(s):
    return s == s[::-1]

# Sliding window substring
def first_k_chars(s, k):
    return s[:k]

# Skip characters
def every_nth_char(s, n):
    return s[::n]
```

## Performance Notes

- **Time:** O(k) where k is slice length
- **Space:** O(k) creates a new string (strings are immutable)
- Slicing creates a copy, doesn't modify original
- Out-of-bounds indices don't raise errors (unlike single index access)

## Problems Using This Method

- [[solutions/strings/encode-decode-strings]] - Extract data segments by length

## Related Methods

- [[methods/built-ins/substring-methods]] - `find()`, `index()`, `split()`
- [[methods/built-ins/string-methods]] - `replace()`, `strip()`, etc.

## Tips and Pitfalls

- **Inclusive start, exclusive stop:** `s[1:4]` gets indices 1, 2, 3 (not 4)
- **Negative indices:** Count from end, `-1` is last character
- **Graceful bounds:** `s[100:200]` on short string returns `""`, no error
- **Immutable:** Slicing creates new string, doesn't modify original
- **Step parameter:** Rarely used but powerful for patterns like reversal
