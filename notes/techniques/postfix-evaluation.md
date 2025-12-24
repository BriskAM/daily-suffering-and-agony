---
tags:
  - technique/stack
  - pattern/expression-parsing
---

# Postfix Evaluation (RPN)

## What is it?

A method to evaluate mathematical expressions where operators follow their operands. Also known as Reverse Polish Notation (RPN). It eliminates the need for parentheses and operator precedence rules.

**Format:** `A B OP` instead of `A OP B`

## When to Use

- "Evaluate Reverse Polish Notation"
- Compilers/Interpreters evaluation phase
- Converting Infix to Postfix (Shunting Yard Algorithm)
- Basic Calculator problems

## Template/Pattern

```python
def eval_rpn(tokens):
    stack = []
    
    for token in tokens:
        if token in operators:
            b = stack.pop()
            a = stack.pop()
            result = apply_op(token, a, b)
            stack.append(result)
        else:
            stack.append(int(token))
            
    return stack[0]
```

## Operator Handling

Be careful with non-commutative operations like `-` and `/`.
- The **first** popped element is the **right** operand (`b`)
- The **second** popped element is the **left** operand (`a`)
- Operation: `a / b` or `a - b`

## Integer Division Pitfall in Python

In Python:
- `//` is **floor division** (rounds down to $-\infty$).
- `int(a / b)` is **truncation** (rounds towards 0).

Most algorithm problems (like LeetCode) expect C++/Java style truncation.

```python
-6 // 132      # -1 (Wrong for RPN usually)
int(-6 / 132)  # 0 (Correct)
```

## Related Techniques

- [[techniques/stack-matching]] - Using stack for structure
- [[techniques/shunting-yard]] - Converting Infix to Postfix

## Problems Using This Technique

- [[solutions/stack-queue/evaluate-reverse-polish-notation]]
