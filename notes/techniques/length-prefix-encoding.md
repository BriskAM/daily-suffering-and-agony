---
tags:
  - technique/serialization
  - pattern/encoding
---

# Length-Prefix Encoding

## What is it?

A serialization technique where each data segment is prefixed with its length before the actual data. This eliminates delimiter collision issues and enables deterministic parsing.

**Format:** `{length}{delimiter}{data}`

## When to Use

- Serializing/deserializing variable-length data
- When data might contain any character (including delimiters)
- Network protocols and file formats
- Encoding lists of strings into a single string
- Any time delimiter escaping would be complex or inefficient

## Template/Pattern

```python
# Encode
def encode(items):
    return ''.join(f'{len(item)}#{item}' for item in items)

# Decode
def decode(s):
    i = 0
    items = []
    while i < len(s):
        # Find delimiter
        j = i
        while s[j] != '#':
            j += 1
        # Extract length
        length = int(s[i:j])
        # Extract data using length
        items.append(s[j+1 : j+1+length])
        # Move to next segment
        i = j + 1 + length
    return items
```

## Key Advantages

| Advantage | Explanation |
|-----------|-------------|
| No escaping needed | Data can contain any character including delimiter |
| Deterministic | Know exactly how many bytes to read |
| Efficient | Single pass for encode/decode |
| Flexible | Works for any data type that can be measured |

## Common Variations

### Variable-Length Encoding
```python
# Length can be variable digits followed by delimiter
"5#hello" → length=5, data="hello"
"100#..." → length=100, data="..."
```

### Fixed-Width Length
```python
# Fixed number of digits for length (e.g., 4 digits)
"0005hello" → length=5, data="hello"
```

**Trade-off:** Fixed-width wastes space but enables seeking without parsing.

## Edge Cases to Handle

```python
# Empty data
encode([""]) → "0#"

# Data containing delimiter
encode(["a#b"]) → "3#a#b"  # Not "a#b" (ambiguous)

# Multiple consecutive delimiters
encode(["##"]) → "2###"

# Large lengths
encode(["x" * 1000]) → "1000#{x repeated 1000 times}"
```

## Problems Using This Technique

- [[solutions/strings/encode-decode-strings]] - Encode list of strings

## Related Techniques

- [[techniques/string-parsing]] - General parsing patterns
- [[techniques/serialization]] - Other serialization methods

## Tips and Pitfalls

- **Delimiter choice:** Use a character unlikely in length numbers (e.g., `#`, `:`, `|`)
- **Integer parsing:** Handle multi-digit lengths correctly
- **Pointer management:** Track position carefully during decode
- **Validation:** Consider validating length doesn't exceed remaining string
- **Empty data:** Zero-length segments are valid: `"0#"`

## Real-World Usage

- **HTTP headers:** `Content-Length` header uses this principle
- **Protocol Buffers:** Variable-length encoding for efficiency
- **Redis protocol:** RESP uses `$length\r\ndata\r\n`
- **Netstrings:** `5:hello,` format by Dan Bernstein
