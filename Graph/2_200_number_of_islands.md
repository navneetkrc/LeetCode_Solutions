# 🌊 Number of Islands – using both BFS & DFS

> Given a 2D grid of `'1'`s (land) and `'0'`s (water), count the number of **disconnected islands**. An island is surrounded by water and connected **horizontally or vertically** (not diagonally).

---

## 🧠 Why All 4 Directions?

In this problem, connectivity is defined **only in four cardinal directions**:

* **Up** `(i-1, j)`
* **Down** `(i+1, j)`
* **Left** `(i, j-1)`
* **Right** `(i, j+1)`

If you skip any of these, the DFS/BFS may miss land cells that are part of the same island but connected in a skipped direction.

### ❌ Example Pitfall

Input:

```python
[
  ["1","1","1"],
  ["0","1","0"],
  ["1","1","1"]
]
```

All land is **connected through vertical and horizontal** paths.

* If we only check right and down → it reports **2 islands** ❌
* If we check all 4 directions → correctly finds **1 island** ✅

---

## ✅ DFS Approach (Depth-First Search)

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        count = 0

        def dfs(i, j):
            if i < 0 or j < 0 or i >= m or j >= n or grid[i][j] != '1':
                return
            grid[i][j] = '-1'  # mark as visited
            dfs(i+1, j)  # down
            dfs(i-1, j)  # up
            dfs(i, j+1)  # right
            dfs(i, j-1)  # left

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    dfs(i, j)

        return count
```

---

## ✅ BFS Approach (Breadth-First Search)

```python
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        count = 0

        def bfs(i, j):
            queue = deque()
            queue.append((i, j))
            grid[i][j] = '-1'  # mark visited

            while queue:
                x, y = queue.popleft()
                for dx, dy in [(1,0), (-1,0), (0,1), (0,-1)]:
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] == '1':
                        grid[nx][ny] = '-1'
                        queue.append((nx, ny))

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    bfs(i, j)

        return count
```

---

## 📌 Summary Table

| Aspect     | DFS                             | BFS                       |
| ---------- | ------------------------------- | ------------------------- |
| Style      | Recursive                       | Iterative (queue-based)   |
| Space      | O(m\*n) in worst case recursion | O(m\*n) for queue         |
| Time       | O(m\*n)                         | O(m\*n)                   |
| Usage      | Simpler code, fast to write     | Safer from stack overflow |
| Directions | Must explore 4 directions       | Must explore 4 directions |

---
