---
tags:
  - method/built-ins
  - python
  - type-conversion
---

# int()

**Module:** Built-in  
**Syntax:** `int(x, base=10)`

## What is it?

Convert a string or number to an integer. Supports multiple numeric bases (binary, octal, hex, etc.).

## When to Use

- Parsing numeric strings from input
- Converting between number systems
- Extracting length/count values from formatted strings
- Type conversion from float (truncates decimal)

## Syntax and Examples

```python
# String to int
int("123")          # 123
int("  42  ")       # 42 - strips whitespace
int("-99")          # -99

# Different bases
int("1010", 2)      # 10 - binary
int("ff", 16)       # 255 - hexadecimal
int("77", 8)        # 63 - octal
int("0b1010", 2)    # 10 - with prefix
int("0xff", 16)     # 255 - with prefix

# Float to int (truncates)
int(3.9)            # 3 - not rounded, truncated
int(-2.7)           # -2

# Boolean to int
int(True)           # 1
int(False)          # 0
```

## Common Operations

| Operation | Syntax | Result |
|-----------|--------|--------|
| Parse decimal | `int("42")` | `42` |
| Parse binary | `int("1010", 2)` | `10` |
| Parse hex | `int("ff", 16)` | `255` |
| Truncate float | `int(3.14)` | `3` |
| Empty string | `int("")` | **ValueError** |

## DSA Patterns

```python
# Parse length from encoded string
def parse_length(s, start):
    end = s.index('#', start)
    length = int(s[start:end])
    return length, end

# Convert digit characters to numbers
def str_to_num(s):
    return int(s)

# Extract number from mixed string
"abc123def".strip("abcdef")  # "123"
int("123")                    # 123

# Base conversion
binary_str = "1010"
decimal = int(binary_str, 2)  # 10
```

## Error Handling

```python
# ValueError for invalid input
try:
    int("abc")      # ValueError: invalid literal
    int("")         # ValueError: invalid literal
    int("12.5")     # ValueError: invalid literal (use float first)
except ValueError:
    # Handle invalid input
    pass

# Valid: handle float conversion
int(float("12.5"))  # 12
```

## Performance Notes

- **Time:** O(n) where n is string length
- **Space:** O(1)
- String parsing is efficient, optimized in CPython

## Problems Using This Method

- [[solutions/strings/encode-decode-strings]] - Parse length prefix

## Related Methods

- [[methods/built-ins/float]] - Convert to float
- [[methods/built-ins/str]] - Convert to string
- [[methods/built-ins/ord-chr]] - Character/ASCII conversion

## Tips and Pitfalls

- **ValueError:** Empty string or invalid characters raise error
- **Truncation:** `int(3.9)` gives `3`, not `4` (use `round()` for rounding)
- **Leading zeros:** `int("007")` is `7` (leading zeros ignored)
- **Whitespace:** Automatically stripped from string edges
- **Base parameter:** Second argument specifies number base (2-36)
- **Float strings:** `int("12.5")` raises error, must convert via `float()` first
