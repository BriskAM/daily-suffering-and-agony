---
tags:
  - difficulty/medium
  - technique/serialization
  - technique/string-parsing
  - pattern/encode-decode
  - company/google
  - company/uber
  - status/solved
---

# Encode and Decode Strings

**Source:** LeetCode #271 (Premium)  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

Implement `encode` and `decode` methods.

**Example:**
```
Input: ["hello", "world"]
encode -> "5#hello5#world"
decode -> ["hello", "world"]
```

**Challenge:** Handle edge cases like strings containing special characters, empty strings, or the delimiter itself.

## Approach

**Primary Technique:** [[notes/techniques/length-prefix-encoding]]  
**Secondary Techniques:** String parsing, slicing

Use **length-prefix encoding**: prefix each string with `{length}#{string}`. This avoids delimiter collision issues since we know exactly how many characters to read.

## Solution

```python
class Solution:
    def encode(self, strs: List[str]) -> str:
        return ''.join(f'{len(s)}#{s}' for s in strs)
    
    def decode(self, s: str) -> List[str]:
        i = 0
        strs = []
        while i < len(s):
            j = i
            # Find the delimiter '#'
            while s[j] != '#':
                j += 1
            # Extract length
            length = int(s[i:j])
            # Extract string using length
            strs.append(s[j+1 : j+1+length])
            # Move to next encoded string
            i = j + 1 + length
        return strs
```

## Complexity Analysis

- **Time Complexity:** 
  - Encode: O(n) where n is total characters across all strings
  - Decode: O(n) single pass through encoded string
- **Space Complexity:** O(n) for output storage

## How It Works

### Encoding
1. For each string `s`, create `{len(s)}#{s}`
2. Join all encoded strings: `"5#hello5#world"`

### Decoding
1. Start at position `i = 0`
2. Find next `#` delimiter at position `j`
3. Extract length: `int(s[i:j])`
4. Extract string: `s[j+1 : j+1+length]`
5. Move pointer: `i = j + 1 + length`
6. Repeat until end of string

### Why This Works

**Delimiter collision avoided:**
- Input: `["4#a", "b"]`
- Bad delimiter approach: `"4#a|b"` → ambiguous
- Length prefix: `"3#4#a1#b"` → unambiguous
  - First string: length=3, content="4#a"
  - Second string: length=1, content="b"

## Edge Cases Handled

| Case | Example | Encoding |
|------|---------|----------|
| Empty string | `[""]` | `"0#"` |
| String with `#` | `["a#b"]` | `"3#a#b"` |
| Multiple `#` | `["##"]` | `"2###"` |
| Empty list | `[]` | `""` |
| Long numbers | `["x" * 100]` | `"100#xxx..."` |

## Alternative Approaches

### Escaped Delimiter (Problematic)

```python
def encode(strs):
    return '|'.join(s.replace('|', '||') for s in strs)
```

**Issue:** Requires escaping, more complex, and longer for strings with many delimiters.

### Fixed-Width Length (Less Flexible)

```python
def encode(strs):
    return ''.join(f'{len(s):04d}{s}' for s in strs)
```

**Issue:** Limits string length (e.g., 4 digits = max 9999 chars). Length-prefix is more flexible.

## Key Insights

- Length-prefix encoding is the standard solution for serialization
- Knowing exact byte count eliminates delimiter ambiguity
- Parse length first, then read exact number of characters
- No escaping needed, handles all characters including delimiter

## Related Problems

- [[solutions/strings/valid-parentheses]] - String parsing pattern
- [[solutions/arrays/group-anagrams]] - String transformation

## Techniques Used

- [[notes/techniques/length-prefix-encoding]]
- [[methods/built-ins/string-slicing]]
- [[methods/built-ins/int]]
