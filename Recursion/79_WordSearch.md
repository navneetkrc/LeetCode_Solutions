
---

# 🔤 Leetcode 79 — Word Search

> **Difficulty:** Medium
> **Topic:** Backtracking
> **Tags:** Matrix, DFS, Recursion

---

## 🧩 Problem Statement

Given an `m x n` grid of characters and a string `word`, return `True` if the word exists in the grid.

* The word can be formed from **sequentially adjacent** cells (up/down/left/right).
* A cell may **not be reused** in the same word.

---

## 🧪 Example Inputs

### ✅ Example 1

```python
board = [
  ["A","B","C","E"],
  ["S","F","C","S"],
  ["A","D","E","E"]
]
word = "ABCCED"
Output: True
```

### ✅ Example 2

```python
word = "SEE"
Output: True
```

### ❌ Example 3

```python
word = "ABCB"
Output: False
```

---

## ✅ Constraints

* `1 <= m, n <= 6`
* `1 <= word.length <= 15`
* `board[i][j]`, `word[k]` ∈ English letters (a-z, A-Z)

---

## 💡 Approach

We'll use **backtracking (DFS)** to try to form the word starting from every cell.

### 🔁 Steps:

1. For each cell in the grid, try to find the word starting from there.
2. At each step:

   * If the character matches the current index of the word:

     * Recurse in all 4 directions (up, down, left, right).
     * Mark the cell as visited (temporarily change the character).
3. If the whole word is matched, return `True`.
4. If no path works, return `False`.

---

## 🧠 Python Code

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        board_length = len(board)
        board_width = len(board[0])
        word_length = len(word)

        #Handle edge case where board has only 1 element
        if board_length == 1 and board_width == 1:
            return board[0][0] == word
        
        def backtrack(i, j, index):
            if index == word_length:
                return True
            
            if board[i][j] != word[index]:
                return False
            
            char = board[i][j] #temp storage for scenario this does not work
            #eg pqabapq -> which a to start with depends on continuity whether we need abaq vs abap

            board[i][j] = "#" # to mark this as visited, and not in future recursive calls

            #check for all 4 directions for word at next index
            for i_off,j_off in [(0,1), (1,0), (0,-1), (-1,0)]:
                r,c = i+i_off, j+j_off

                #ensure next values are within boundaries of board
                if 0<=r<board_length and 0<=c<board_width:
                    if backtrack(r,c,index+1): #check recusrively for char at next index
                        return True  

            board[i][j] = char #reset char to previous value and mark as unused
            return False

        # Recursively check for all the starting points in the board

        for i in range(board_length):
            for j in range(board_width):
                if backtrack(i,j,0):
                    return True
        return False

```
---

## 🧠 Python Code (Well-Commented)

```python
from typing import List

class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows = len(board)
        cols = len(board[0])
        word_length = len(word)

        def dfs(row: int, col: int, index: int) -> bool:
            # If all characters are matched
            if index == word_length:
                return True

            # If out of bounds or current cell doesn't match
            if not (0 <= row < rows and 0 <= col < cols) or board[row][col] != word[index]:
                return False

            # Save current cell's character and mark as visited
            original_char = board[row][col]
            board[row][col] = "#"

            # Try all 4 directions
            for row_offset, col_offset in [(0,1), (1,0), (0,-1), (-1,0)]:
                next_row = row + row_offset
                next_col = col + col_offset

                if dfs(next_row, next_col, index + 1):
                    return True  # Found a valid path

            # Backtrack — reset the cell
            board[row][col] = original_char
            return False

        # Try to find the word starting from every cell
        for i in range(rows):
            for j in range(cols):
                if dfs(i, j, 0):
                    return True

        return False
```

---

## 🔍 Trace for Example 1: `"ABCCED"`

1. Start at (0,0) → `'A'` matches
2. Move to (0,1) → `'B'` matches
3. Move to (0,2) → `'C'` matches
4. Move down to (1,2) → `'C'` matches
5. Move down to (2,2) → `'E'` matches
6. Move left to (2,1) → `'D'` matches ✅

---

## 🧮 Time & Space Complexity

| Complexity | Value                                            |
| ---------- | ------------------------------------------------ |
| Time       | O(M \* N \* 4^L) — worst case: explore all paths |
| Space      | O(L) — max recursion stack (length of word)      |

Where:

* `M` × `N` is the board size
* `L` is the word length

---

## 🧠 Key Concepts

* ✅ Backtracking
* ✅ Depth-First Search (DFS)
* ✅ Grid Traversal
* ✅ State Restoration (undo visited marks)

---

🎨 Visual Representation
Search Pattern for each cell:
    ↑
    |
← cell →
    |
    ↓

Backtracking Process:
Try → Mark → Explore → Backtrack → Try Next

---
