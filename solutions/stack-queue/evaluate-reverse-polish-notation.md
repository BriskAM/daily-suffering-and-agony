---
tags:
  - difficulty/medium
  - technique/stack
  - pattern/postfix-evaluation
  - company/amazon
  - company/google
  - company/microsoft
  - status/solved
---

# Evaluate Reverse Polish Notation

**Source:** LeetCode #150  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Evaluate the value of an arithmetic expression in **Reverse Polish Notation** (RPN).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression. The division between two integers always **truncates toward zero**.

**Example:**
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

## Approach

**Primary Technique:** [[notes/techniques/postfix-evaluation]] (Stack)

Reverse Polish Notation (Postfix) is designed to be evaluated with a stack:
1. Iterate through tokens.
2. If token is a **number**, push to stack.
3. If token is an **operator**, pop top two elements, apply operation, and push result back.

**Order Matters:**
For `stack = [a, b]` and operator `-`:
- `b` is popped first (top)
- `a` is popped second
- Operation is `a - b`

**Division Detail:**
Python's `//` operator floors (truncates toward negative infinity), but the problem requires truncation toward zero.
- `6 // -132` → `-1` (problem expects `0` as `int(6/-132)`)
- Solution: Use `int(a / b)` for correct truncation toward zero.

## Solution

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        
        for t in tokens:
            if t not in {"+", "-", "*", "/"}:
                # Push numbers to stack
                stack.append(int(t))
            else:
                # Operator: pop last two operands
                b = stack.pop()
                a = stack.pop()
                
                if t == "+":
                    stack.append(a + b)
                elif t == "-":
                    stack.append(a - b)
                elif t == "*":
                    stack.append(a * b)
                elif t == "/":
                    # int(a / b) truncates toward zero
                    stack.append(int(a / b))
        
        return stack[0]
```

## Complexity Analysis

- **Time Complexity:** O(n) - Single pass through tokens.
- **Space Complexity:** O(n) - Stack can store up to n/2 operands.

## How It Works

**Example:** `["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]`

1. Push 10, 6, 9, 3
   Stack: `[10, 6, 9, 3]`
2. Op `+`: `9 + 3 = 12`
   Stack: `[10, 6, 12]`
3. Op `*` (next is -11? no, example continues): `12 * -11` ...

**Simple Example:** `["4", "13", "5", "/", "+"]`
1. Push 4: `[4]`
2. Push 13: `[4, 13]`
3. Push 5: `[4, 13, 5]`
4. Op `/`: Pop 5 (b), 13 (a) → `13 / 5 = 2.6` → `int(2.6) = 2`
   Stack: `[4, 2]`
5. Op `+`: Pop 2 (b), 4 (a) → `4 + 2 = 6`
   Stack: `[6]`
6. Return 6

## Edge Cases

- **Negative Division:** `int(-6 / 132)` = 0 vs `-6 // 132` = -1. Use `int()` cast.
- **Single Element:** Stack just returns the element.
- **Negative numbers:** Helper `int()` handles negative strings correctly.

## Related Problems

- [[solutions/stack-queue/valid-parentheses]] - Stack usage pattern
- [[solutions/stack-queue/min-stack]] - Stack usage pattern
- [[solutions/stack-queue/generate-parentheses]] - Expression generation

## Techniques Used

- [[notes/techniques/postfix-evaluation]]
- [[notes/techniques/stack-matching]]
