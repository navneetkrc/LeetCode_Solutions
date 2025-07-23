# 🌊 Leetcode 695: Max Area of Island

## 📝 Problem Description

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

 

Example 1:
---
<img width="1053" height="653" alt="image" src="https://github.com/user-attachments/assets/1898e59a-ec02-4edb-83e4-2cb4aa2d4ed8" />

---


Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]

**Output: 6**

**Explanation:** The answer is not 11, because the island must be connected 4-directionally.


**Example 2:**

Input: grid = [[0,0,0,0,0,0,0,0]]

Output: 0
 

**Constraints:**

m == grid.length
n == grid[i].length
1 <= m, n <= 50
grid[i][j] is either 0 or 1.

---
### 🔍 Constraints:
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`

---

## 💡 Intuition & Key Observations

- Each `'1'` is a land tile. A connected component of `'1'`s represents **an island**.
- Use **DFS or BFS** to explore each island and calculate its area.
- Keep track of the **maximum area** encountered during the traversal.

---

## 🎯 Interview Expectations

### ✅ What interviewers look for:
- Do you recognize this as a **graph traversal** problem?
- Can you **safely mark visited nodes** to avoid cycles?
- Are you reusing logic across recursive or iterative calls?
- Are your **variable names self-explanatory**?
- Do you **communicate** base conditions, loop structure, and corner cases clearly?

---

## ✅ Approach 1: Depth-First Search (DFS)

### 🔧 Idea:
Recursively explore in 4 directions and **return area = 1 + sum of neighbors**.

### 💬 Interview Talking Points:
- “We visit a land cell and explore all reachable adjacent land cells.”
- “We mark visited land to avoid recounting.”
- “We use recursion to accumulate island size.”

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        
        def dfs(row: int, col: int) -> int:
            # Boundary or water check
            if row < 0 or row >= rows or col < 0 or col >= cols or grid[row][col] != 1:
                return 0
            
            grid[row][col] = -1  # Mark cell as visited
            # Explore all 4 directions and return cumulative area
            return (1 +
                    dfs(row + 1, col) +
                    dfs(row - 1, col) +
                    dfs(row, col + 1) +
                    dfs(row, col - 1))
        
        max_island_area = 0
        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == 1:
                    island_area = dfs(row, col)
                    max_island_area = max(max_island_area, island_area)

        return max_island_area
````

### 🧪 Time & Space Complexity:

* **Time**: `O(m × n)` — each cell is visited once
* **Space**: `O(m × n)` — call stack depth in worst case (for DFS recursion)

---

## 🔁 Approach 2: Breadth-First Search (BFS)

### 💡 Use a queue to simulate level-wise exploration

```python
from collections import deque

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        max_area = 0
        
        def bfs(start_row: int, start_col: int) -> int:
            queue = deque()
            queue.append((start_row, start_col))
            area = 0
            grid[start_row][start_col] = -1  # mark visited
            
            while queue:
                row, col = queue.popleft()
                area += 1
                for dr, dc in [(1,0), (-1,0), (0,1), (0,-1)]:
                    r, c = row + dr, col + dc
                    if 0 <= r < rows and 0 <= c < cols and grid[r][c] == 1:
                        queue.append((r, c))
                        grid[r][c] = -1  # mark visited
            return area

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == 1:
                    max_area = max(max_area, bfs(row, col))
        
        return max_area
```

### 🧪 Time & Space Complexity:

* **Time**: `O(m × n)`
* **Space**: `O(min(m × n, queue size))`

---

## 🔍 Sample Test Case

```python
Input:
grid = [
  [0,0,1,0,0,0,0],
  [0,0,1,1,1,0,0],
  [0,0,0,0,1,0,0],
  [0,1,1,0,0,0,0]
]

Output:
6
```

---

## 🧠 Summary: What You Should Explain in an Interview

| Concept       | Explanation                                                         |
| ------------- | ------------------------------------------------------------------- |
| Traversal     | Use DFS or BFS to explore all land tiles connected to a given cell. |
| Visited Cells | Use `-1` or a visited set to avoid revisiting cells.                |
| Area Tracking | Accumulate area during each DFS/BFS call and update max.            |
| Edge Handling | Always validate grid boundaries before accessing neighbors.         |
| Clean Code    | Use descriptive variable names like `row`, `col`, `area`.           |

---

## ✅ Final Tips

* Prefer DFS for simplicity, BFS for iterative clarity (especially in constrained recursion depth).
* Avoid mutating the grid in-place if interviewer specifies immutability (use a copy).
* Always clarify assumptions and constraints with the interviewer.

---

🧠 **Practice saying:**

> "Each island is a connected component. I perform DFS to explore its area and track the maximum island size across all components."

---
