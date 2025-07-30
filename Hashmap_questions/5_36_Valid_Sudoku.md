# ✅ Valid Sudoku – Multiple Approaches

---

### 🔹 Approach 1: **Separate Validation for Rows, Columns, and 3×3 Boxes (Using Sets)**

> Use three separate checks — one each for rows, columns, and sub-boxes — to ensure all contain only unique digits.

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Validate Rows
        for i in range(9):
            s = set()
            for j in range(9):
                item = board[i][j]
                if item in s:
                    return False
                elif item != '.':
                    s.add(item)
        
        # Validate Cols
        for i in range(9):
            s = set()
            for j in range(9):
                item = board[j][i]
                if item in s:
                    return False
                elif item != '.':
                    s.add(item)
            
        # Validate Boxes
        starts = [(0, 0), (0, 3), (0, 6),
                  (3, 0), (3, 3), (3, 6),
                  (6, 0), (6, 3), (6, 6)]
        
        for i, j in starts:
            s = set()
            for row in range(i, i+3):
                for col in range(j, j+3):
                    item = board[row][col]
                    if item in s:
                        return False
                    elif item != '.':
                        s.add(item)
        return True

# Time Complexity: O(n^2)
# Space Complexity: O(n)
```

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Validate rows
        for i in range(9):
            seen = set()
            for j in range(9):
                val = board[i][j]
                if val in seen:
                    return False
                if val != '.':
                    seen.add(val)
        
        # Validate columns
        for i in range(9):
            seen = set()
            for j in range(9):
                val = board[j][i]
                if val in seen:
                    return False
                if val != '.':
                    seen.add(val)
        
        # Validate 3x3 sub-boxes
        for box_row in [0, 3, 6]:
            for box_col in [0, 3, 6]:
                seen = set()
                for i in range(box_row, box_row + 3):
                    for j in range(box_col, box_col + 3):
                        val = board[i][j]
                        if val in seen:
                            return False
                        if val != '.':
                            seen.add(val)
        
        return True
```

* **Time Complexity:** `O(9²)` = `O(1)` (since the board size is constant)
* **Space Complexity:** `O(9)` for each row/col/box set

---

### 🔹 Approach 2: **Single Pass with Dictionaries for Row, Column, and Box Checks**

> Use a single pass through the board and track occurrences of numbers in rows, columns, and boxes simultaneously using dictionaries.

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        from collections import defaultdict

        rows = defaultdict(set)
        cols = defaultdict(set)
        boxes = defaultdict(set)

        for r in range(9):
            for c in range(9):
                val = board[r][c]
                if val == '.':
                    continue
                
                box_index = (r // 3, c // 3)
                
                if (val in rows[r]) or (val in cols[c]) or (val in boxes[box_index]):
                    return False
                
                rows[r].add(val)
                cols[c].add(val)
                boxes[box_index].add(val)

        return True
```

* **Time Complexity:** `O(81)` = `O(1)`
* **Space Complexity:** `O(27)` for dictionaries (9 rows + 9 cols + 9 boxes)

---

### 🔹 Approach 3: **Bitmasking (Optimized Space Use)**

> Use bit manipulation to track the existence of digits in rows, columns, and boxes with integers (bits 1–9).

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rows = [0] * 9
        cols = [0] * 9
        boxes = [0] * 9
        
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    continue
                
                val = int(board[i][j])
                bitmask = 1 << val

                box_index = (i // 3) * 3 + j // 3

                if (rows[i] & bitmask) or (cols[j] & bitmask) or (boxes[box_index] & bitmask):
                    return False
                
                rows[i] |= bitmask
                cols[j] |= bitmask
                boxes[box_index] |= bitmask

        return True
```

* **Time Complexity:** `O(1)`
* **Space Complexity:** `O(1)` (constant 9-length arrays)

---

### 🔹 Approach 4: **Flattened Coordinates with Tuples in a Set**

> Store each digit’s location using a `(digit, row)`, `(digit, col)`, and `(digit, box)` tuple to detect duplicates.

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        seen = set()

        for i in range(9):
            for j in range(9):
                num = board[i][j]
                if num == '.':
                    continue
                if ((num, i) in seen or 
                    (num, j + 9) in seen or 
                    (num, (i//3, j//3)) in seen):
                    return False
                
                seen.add((num, i))
                seen.add((num, j + 9))
                seen.add((num, (i//3, j//3)))

        return True
```

* **Time Complexity:** `O(1)`
* **Space Complexity:** `O(1)` (limited by 81 cells)

---

### ℹ️ Summary Table:

| Approach Name             | Description                                  | Space | Time |
| ------------------------- | -------------------------------------------- | ----- | ---- |
| Separate Set Checks       | Validate rows, columns, and boxes one-by-one | O(n)  | O(1) |
| Single-Pass Dict Tracking | All checks in one loop using dictionaries    | O(n)  | O(1) |
| Bitmasking                | Optimized space using integer bit masks      | O(1)  | O(1) |
| Coordinate Tuples in Set  | Flattened keys using tuples                  | O(n)  | O(1) |

