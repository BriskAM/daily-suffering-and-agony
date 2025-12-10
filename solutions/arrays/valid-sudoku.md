---
tags:
  - difficulty/medium
  - technique/matrix-validation
  - technique/frequency-count
  - pattern/2d-array
  - company/google
  - company/amazon
  - company/apple
  - status/solved
---

# Valid Sudoku

**Source:** LeetCode #36  
**Difficulty:** Medium  
**Date Solved:** 2025-12-10

## Problem Statement

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain digits 1-9 without repetition
2. Each column must contain digits 1-9 without repetition
3. Each of the nine 3×3 sub-boxes must contain digits 1-9 without repetition

**Note:** 
- A partially filled board can be valid
- Empty cells are denoted by `'.'`

**Example:**
```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]

Output: true
```

## Approach

**Primary Technique:** [[notes/techniques/matrix-validation]]  
**Secondary Techniques:** [[notes/techniques/frequency-count]], [[methods/collections/counter]]

Validate three constraints separately:
1. **Rows:** Check each row for duplicate digits
2. **Columns:** Check each column for duplicate digits  
3. **3×3 Boxes:** Check each 3×3 sub-grid for duplicate digits

Use `Counter` to detect duplicates: if any count > 1, invalid.

## Solution (Initial Implementation)

```python
from collections import Counter

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Check rows
        for row in board:
            nums = [i for i in row if i != '.']
            count = Counter(nums)
            for c in count.values():
                if c > 1:
                    return False
        
        # Check columns
        for col in range(len(board)):
            nums = [board[i][col] for i in range(len(board)) if board[i][col] != '.']
            count = Counter(nums)
            for c in count.values():
                if c > 1:
                    return False
        
        # Check 3x3 boxes
        for br in range(0, 9, 3):
            for bc in range(0, 9, 3):
                box = []
                for r in range(br, br + 3):
                    for c in range(bc, bc + 3):
                        if board[r][c] != '.':
                            box.append(board[r][c])
                if any(count > 1 for count in Counter(box).values()):
                    return False
        
        return True
```

## Complexity Analysis

- **Time Complexity:** O(1) - Always 9×9 board = 81 cells, constant work
  - Rows: 9 iterations × 9 cells = O(81)
  - Columns: 9 iterations × 9 cells = O(81)
  - Boxes: 9 boxes × 9 cells = O(81)
  - Total: O(243) = O(1)
- **Space Complexity:** O(1) - Counter stores at most 9 digits

## How It Works

### 1. Row Validation
```python
for row in board:
    nums = [i for i in row if i != '.']  # Filter out empty cells
    count = Counter(nums)                # Count frequencies
    if any(c > 1 for c in count.values()):  # Any duplicates?
        return False
```

### 2. Column Validation
```python
for col in range(9):
    nums = [board[i][col] for i in range(9) if board[i][col] != '.']
    # Same duplicate check
```

### 3. Box Validation
```python
# Iterate through 3x3 box starting positions: (0,0), (0,3), (0,6), (3,0), ...
for br in range(0, 9, 3):      # Box row: 0, 3, 6
    for bc in range(0, 9, 3):  # Box col: 0, 3, 6
        # Extract all cells in this 3x3 box
        box = []
        for r in range(br, br + 3):
            for c in range(bc, bc + 3):
                if board[r][c] != '.':
                    box.append(board[r][c])
        # Check for duplicates
```

## Optimized Approaches

### Single Pass with Sets (More Efficient)

```python
def isValidSudoku(board):
    seen = set()
    for i in range(9):
        for j in range(9):
            num = board[i][j]
            if num != '.':
                # Unique encoding for row, column, box
                row_key = f"{num} in row {i}"
                col_key = f"{num} in col {j}"
                box_key = f"{num} in box {i//3}-{j//3}"
                
                if row_key in seen or col_key in seen or box_key in seen:
                    return False
                
                seen.add(row_key)
                seen.add(col_key)
                seen.add(box_key)
    return True
```

**Advantages:**
- Single pass through the board
- No repeated Counter creation
- More concise

### Using Hash Sets for Each Constraint

```python
def isValidSudoku(board):
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    for i in range(9):
        for j in range(9):
            num = board[i][j]
            if num != '.':
                box_idx = (i // 3) * 3 + (j // 3)
                
                if num in rows[i] or num in cols[j] or num in boxes[box_idx]:
                    return False
                
                rows[i].add(num)
                cols[j].add(num)
                boxes[box_idx].add(num)
    return True
```

**Advantages:**
- Single pass
- O(1) lookup with sets
- Clean separation of concerns

## Key Insights

- **Counter for validation:** Natural way to detect duplicates
- **Box indexing:** `(row // 3) * 3 + (col // 3)` maps cell to box number (0-8)
- **Filter empties:** Skip `'.'` cells when building frequency maps
- **Constant complexity:** Fixed 9×9 size means O(1) time/space
- **Early termination:** Return `False` immediately on first invalid constraint

## Edge Cases Handled

| Case | Example | Validation |
|------|---------|------------|
| Empty board | All `'.'` | Valid (no duplicates) |
| Single row invalid | `["1","1",...]` | Fails row check |
| Column invalid | Column has duplicate | Fails column check |
| Box invalid | 3×3 box has duplicate | Fails box check |
| Valid partial | Some filled, no duplicates | Valid |

## Related Problems

- [[solutions/arrays/contains-duplicate]] - Basic duplicate detection
- [[solutions/arrays/group-anagrams]] - Using Counter for grouping

## Techniques Used

- [[notes/techniques/matrix-validation]]
- [[notes/techniques/frequency-count]]
- [[methods/collections/counter]]
- [[methods/built-ins/any]]
