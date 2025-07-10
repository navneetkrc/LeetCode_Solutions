## ✅ Problem Recap

You're given a `grid` of `'1'`s (land) and `'0'`s (water), and you must count the number of **disconnected islands**, where:

* Islands are groups of **horizontally or vertically** connected `'1'`s.
* Diagonals **do not** connect islands.

---

## 🧠 Core Intuition Behind DFS

When you find a `'1'`, you:

1. Say: **"This is a new island."** → `count += 1`
2. Then: **Explore all land connected to it** in 4 directions (up, down, left, right).
3. While doing so, **mark visited `'1'`s as `'0'`** so we don't count them again.

This is **flood fill** — once you discover an island, you flood through all its parts to eliminate future duplication.

---

## 🧭 Visual Walkthrough with Example

```python
grid = [
  ["1", "1", "0", "0", "0"],
  ["1", "1", "0", "0", "0"],
  ["0", "0", "1", "0", "0"],
  ["0", "0", "0", "1", "1"]
]
```

### Step-by-step:

### 🔹 Start scanning:

We loop `i` from `0` to `3`, and `j` from `0` to `4`.

#### 1️⃣ `grid[0][0] == '1'` → First island

We:

* Increase `count = 1`
* Call `dfs(0, 0)`
* It recursively marks all connected `'1'`s:

  * `grid[0][1]`, `grid[1][0]`, `grid[1][1]`
* All these are now `'0'` (visited)

#### 2️⃣ Continue... all 0s until:

#### 3️⃣ `grid[2][2] == '1'` → Second island

* Increase `count = 2`
* Call `dfs(2, 2)` → only `grid[2][2]` gets marked

#### 4️⃣ `grid[3][3] == '1'` → Third island

* Increase `count = 3`
* Call `dfs(3, 3)` → marks `grid[3][4]` too

Done! ✅

---

## 🔄 DFS Call Stack (How It Works)

Example when visiting `(0,0)`:

```text
dfs(0,0)
→ grid[0][0] = '0'
→ dfs(1,0)
   → grid[1][0] = '0'
   → dfs(2,0) → grid[2][0] = '0' → base case (already 0)
   → dfs(1,1)
       → grid[1][1] = '0'
       → dfs(0,1)
           → grid[0][1] = '0'
```

The DFS visits all connected land and marks them `'0'`.

---

## 🧱 Final Grid (After All DFS)

```python
[
  ["0", "0", "0", "0", "0"],
  ["0", "0", "0", "0", "0"],
  ["0", "0", "0", "0", "0"],
  ["0", "0", "0", "0", "0"]
]
```

---

## ✅ Final Count: `3 islands`

---

## 💡 Why DFS Is Powerful:

| Feature                | Why it's Good                                         |
| ---------------------- | ----------------------------------------------------- |
| ✅ Visited Marking      | Prevents duplicate counting                           |
| ✅ Recursive            | Explores all directions simply                        |
| ✅ Works for All Shapes | L, Z, donut, scattered blobs                          |
| ✅ Space-efficient      | No need for extra visited matrix if you mark in-place |

---

## ✏️ Final Annotated Code

```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        count = 0

        def dfs(r, c):
            # Boundary or water
            if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != '1':
                return
            grid[r][c] = '0'  # Mark visited
            # Explore all 4 directions
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        # Scan every cell
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == '1':
                    count += 1  # New island
                    dfs(i, j)   # Flood it

        return count
```

---
