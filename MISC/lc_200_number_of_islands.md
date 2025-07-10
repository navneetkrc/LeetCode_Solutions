# 🌊 Leetcode 200: Number of Islands

## 🧩 Problem Description

You are given an `m x n` 2D binary grid that represents a map of `'1'`s (land) and `'0'`s (water).  
Return **the number of islands**.

An **island** is formed by connecting **adjacent lands** (connected **horizontally or vertically**, not diagonally).  
You may assume **all four edges** of the grid are surrounded by water.

### 🧪 Examples

#### Example 1:
```

Input:
grid = \[
\["1","1","1","1","0"],
\["1","1","0","1","0"],
\["1","1","0","0","0"],
\["0","0","0","0","0"]
]
Output: 1

```

#### Example 2:
```

Input:
grid = \[
\["1","1","0","0","0"],
\["1","1","0","0","0"],
\["0","0","1","0","0"],
\["0","0","0","1","1"]
]
Output: 3

````

---

## ✅ Constraints

- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`

---

## 💡 Interview Expectations

### 🔍 What interviewers look for:
- ✅ Recognizing grid traversal as a **graph traversal problem**.
- ✅ Choosing the right technique (DFS, BFS, or Union-Find).
- ✅ Handling **visited cells** properly to avoid reprocessing.
- ✅ Writing clean, modular code with proper **edge condition checks**.
- ✅ Explaining **space vs time trade-offs** clearly.

---

## 🚀 Approach 1: Depth-First Search (DFS)

### 🧠 Intuition
Think of every `'1'` you find as **starting a new island**. Once found, **explore all its neighbors** using DFS and **mark them visited**. Repeat for every unvisited `'1'`.

### 🔧 Key Observations
- Grid traversal using DFS
- Mark visited cells by modifying the original grid or using a `visited` matrix
- Check **4 directions**: up, down, left, right

---

💡 Why DFS Is Powerful:
Feature	Why it's Good

✅ Visited Marking	Prevents duplicate counting

✅ Recursive	Explores all directions simply

✅ Works for All Shapes	L, Z, donut, scattered blobs

✅ Space-efficient	No need for extra visited matrix if you mark in-place


---

### ✅ Clean & Concise DFS Code (In-Place)

```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        island_count = 0

        def dfs(r: int, c: int):
            # Base case: Out of bounds or water or already visited
            if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] != '1':
                return
            # Mark the land as visited
            grid[r][c] = '#'
            # Explore all four directions
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        # Traverse the grid
        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == '1':
                    island_count += 1
                    dfs(row, col)

        return island_count
````

---

### 📊 Time and Space Complexity

| Metric           | Value                                  |
| ---------------- | -------------------------------------- |
| Time Complexity  | O(m × n) — visit every cell once       |
| Space Complexity | O(m × n) in worst-case recursion stack |

---

## 🔁 Approach 2: Breadth-First Search (BFS)

### 🔄 When to prefer BFS

* Interviewer explicitly asks for BFS
* You want **iterative solution** (no recursion depth limits)

```python
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        island_count = 0
        directions = [(0,1), (0,-1), (1,0), (-1,0)]

        def bfs(r, c):
            queue = deque()
            queue.append((r, c))
            grid[r][c] = '#'

            while queue:
                row, col = queue.popleft()
                for dr, dc in directions:
                    new_r, new_c = row + dr, col + dc
                    if (0 <= new_r < rows and 0 <= new_c < cols and grid[new_r][new_c] == '1'):
                        grid[new_r][new_c] = '#'
                        queue.append((new_r, new_c))

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == '1':
                    island_count += 1
                    bfs(row, col)

        return island_count
```

---

## 🔗 Approach 3: Union-Find (Disjoint Set Union)

### 🧠 Intuition

Each land cell is a node. Initially, each is its own island. If adjacent lands are connected, we merge them into one set.

✅ More optimal if:

* You need to **handle dynamic updates** (e.g., adding/removing land)

```python
class UnionFind:
    def __init__(self, grid):
        self.parent = {}
        self.count = 0
        rows, cols = len(grid), len(grid[0])
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    idx = (r, c)
                    self.parent[idx] = idx
                    self.count += 1

    def find(self, node):
        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, a, b):
        rootA, rootB = self.find(a), self.find(b)
        if rootA != rootB:
            self.parent[rootB] = rootA
            self.count -= 1

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        uf = UnionFind(grid)
        rows, cols = len(grid), len(grid[0])
        directions = [(0,1), (1,0)]

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    for dr, dc in directions:
                        nr, nc = r + dr, c + dc
                        if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == '1':
                            uf.union((r, c), (nr, nc))

        return uf.count
```

---

## 🗣️ What to Say in the Interview

### ✅ While thinking:

* “I’m thinking of using DFS or BFS since we’re working with adjacent nodes in a grid.”
* “Each island can be seen as a connected component.”

### ✅ While coding:

* “I’m marking visited cells by modifying the grid itself to avoid extra space.”
* “I'll explore 4-directionally — not diagonals — since only vertical and horizontal are allowed.”

### ✅ After coding:

* “This handles edge cases like isolated '1's or fully water grids.”
* “We could switch to Union-Find for dynamic updates if needed.”

---

## ⚠️ Common Pitfalls

* ❌ Modifying original grid when not allowed (e.g., interview constraints)
* ❌ Forgetting to bound check in DFS/BFS
* ❌ Visiting diagonals when not allowed

---

## 🔄 Follow-up Variations

* 🔁 Count number of lakes (0's surrounded by 1s)
* ⬜ Return size of largest island
* 🔧 Convert surrounded regions to water (`Surrounded Regions` problem)
* 🌍 Number of provinces (similar to islands in adjacency matrix)

---

## 📌 Summary

| Approach   | Space    | Time             | Notes                              |
| ---------- | -------- | ---------------- | ---------------------------------- |
| DFS        | O(1)     | O(m × n)         | Easy to write, clean recursion     |
| BFS        | O(m × n) | O(m × n)         | Uses queue, avoids recursion limit |
| Union-Find | O(m × n) | O(m × n \* α(n)) | Great for dynamic connectivity     |

---

✅ Pick DFS as your first go-to approach

✅ Be ready to switch if recursion depth or space is a concern

✅ Practice Union-Find if asked about performance or dynamic connectivity

---
