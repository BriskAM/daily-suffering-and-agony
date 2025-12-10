---
tags:
  - technique/validation
  - pattern/2d-array
---

# Matrix Validation

## What is it?

Techniques for validating constraints across rows, columns, and sub-regions of a 2D matrix. Common in grid-based problems like Sudoku, game boards, and matrix puzzles.

## When to Use

- Sudoku validation
- Tic-tac-toe win detection
- Chess move validation
- Grid constraint checking
- Matrix property verification (symmetry, uniqueness, etc.)

## Common Patterns

### 1. Row Validation

```python
# Check each row independently
for row in matrix:
    # Validate row constraint
    if not is_valid_row(row):
        return False
```

### 2. Column Validation

```python
# Extract and check each column
for col in range(len(matrix[0])):
    column = [matrix[row][col] for row in range(len(matrix))]
    if not is_valid_column(column):
        return False
```

### 3. Sub-Grid Validation (e.g., 3×3 boxes in Sudoku)

```python
# Iterate through box starting positions
for box_row in range(0, n, box_size):
    for box_col in range(0, n, box_size):
        box = []
        for r in range(box_row, box_row + box_size):
            for c in range(box_col, box_col + box_size):
                box.append(matrix[r][c])
        if not is_valid_box(box):
            return False
```

### 4. Single-Pass Validation (Optimized)

```python
# Check all constraints in one pass
rows = [set() for _ in range(n)]
cols = [set() for _ in range(n)]
boxes = [set() for _ in range(num_boxes)]

for i in range(n):
    for j in range(n):
        value = matrix[i][j]
        box_idx = (i // box_size) * boxes_per_row + (j // box_size)
        
        # Check all three constraints
        if value in rows[i] or value in cols[j] or value in boxes[box_idx]:
            return False
        
        rows[i].add(value)
        cols[j].add(value)
        boxes[box_idx].add(value)
```

## Box/Sub-Grid Indexing

For a 9×9 Sudoku board divided into 3×3 boxes:

```
Box indices:
0 0 0 | 1 1 1 | 2 2 2
0 0 0 | 1 1 1 | 2 2 2
0 0 0 | 1 1 1 | 2 2 2
------+-------+------
3 3 3 | 4 4 4 | 5 5 5
3 3 3 | 4 4 4 | 5 5 5
3 3 3 | 4 4 4 | 5 5 5
------+-------+------
6 6 6 | 7 7 7 | 8 8 8
6 6 6 | 7 7 7 | 8 8 8
6 6 6 | 7 7 7 | 8 8 8

Formula: box_index = (row // 3) * 3 + (col // 3)
```

**Examples:**
- Cell (0, 0) → box 0: `(0//3)*3 + (0//3) = 0`
- Cell (4, 5) → box 4: `(4//3)*3 + (5//3) = 1*3 + 1 = 4`
- Cell (8, 8) → box 8: `(8//3)*3 + (8//3) = 2*3 + 2 = 8`

## Duplicate Detection Methods

### Using Counter

```python
from collections import Counter

def has_duplicates(items):
    count = Counter(items)
    return any(c > 1 for c in count.values())
```

### Using Sets (More Efficient)

```python
def has_duplicates(items):
    return len(items) != len(set(items))
```

### Using Hash Set (Single Pass)

```python
def has_duplicates(items):
    seen = set()
    for item in items:
        if item in seen:
            return True
        seen.add(item)
    return False
```

## Optimization Strategies

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Three separate passes | O(3n²) | O(n) | Simple but redundant |
| Single pass with sets | O(n²) | O(n²) | Optimal for most cases |
| Early termination | Varies | O(n²) | Best case < O(n²) |

## Common Variants

### Diagonal Validation

```python
# Main diagonal
main_diag = [matrix[i][i] for i in range(n)]

# Anti-diagonal
anti_diag = [matrix[i][n-1-i] for i in range(n)]
```

### Symmetric Validation

```python
# Check if matrix equals its transpose
def is_symmetric(matrix):
    n = len(matrix)
    for i in range(n):
        for j in range(i+1, n):
            if matrix[i][j] != matrix[j][i]:
                return False
    return True
```

## Problems Using This Technique

- [[solutions/arrays/valid-sudoku]] - Validate rows, columns, and 3×3 boxes

## Related Techniques

- [[techniques/frequency-count]] - Duplicate detection
- [[techniques/hashset-lookup]] - Fast membership testing
- [[techniques/2d-array-traversal]] - Matrix iteration patterns

## Tips and Pitfalls

- **Box indexing:** Master the formula `(row // box_size) * boxes_per_row + (col // box_size)`
- **Filter empties:** Skip placeholder values (e.g., `'.'` in Sudoku)
- **Single pass:** Use multiple data structures to validate all constraints simultaneously
- **Early exit:** Return `False` immediately on first violation
- **Fixed size:** For fixed-size grids (e.g., 9×9), complexity is O(1)
