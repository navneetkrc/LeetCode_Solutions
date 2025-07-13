# 🧭 Leetcode 62: Unique Paths

---

## 📘 Problem Description

A robot is located at the top-left corner of an `m x n` grid.  
The robot can only move **either down or right** at any point in time.

Return the total number of **unique paths** that the robot can take to reach the **bottom-right** corner.

---

### 🧠 Example 1:
```

Input: m = 3, n = 7
Output: 28

```

### 🧠 Example 2:
```

Input: m = 3, n = 2
Output: 3
Explanation:
→→↓, →↓→, ↓→→ are the 3 paths.

```

---

## 🔍 Constraints

- `1 <= m, n <= 100`
- The answer is guaranteed to be less than `2 * 10⁹`

---

## 💡 Intuition

To reach cell `(i, j)`, you could come from:
- The **cell above**: `(i-1, j)`
- The **cell to the left**: `(i, j-1)`

So,
```

dp\[i]\[j] = dp\[i-1]\[j] + dp\[i]\[j-1]

````

You initialize the **first row and first column** with `1` because there's **only one way** to reach those cells (all rights or all downs).

---

## ✅ Approach 1: 2D DP Table (Tabulation)

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Initialize the grid with all 1s, as only one way to reach any cell in first row/column
        dp = [[1 for _ in range(n)] for _ in range(m)]

        # Fill the DP table using bottom-up approach
        for row in range(1, m):
            for col in range(1, n):
                dp[row][col] = dp[row - 1][col] + dp[row][col - 1]

        # The destination is the bottom-right corner
        return dp[m - 1][n - 1]
````

### 🧪 Dry Run: m = 3, n = 3

| i/j | 0 | 1 | 2 |
| --- | - | - | - |
| 0   | 1 | 1 | 1 |
| 1   | 1 | 2 | 3 |
| 2   | 1 | 3 | 6 |

✅ Answer: `6` unique paths

---

### ⏱ Time and Space Complexity

* **Time:** O(m × n)
* **Space:** O(m × n)

---

## ✅ Approach 2: Space Optimized DP (Using 1D Array)

Since each row only depends on the **previous row**, we can reduce the space to **O(n)** using a 1D array.

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Initialize a single row with 1s
        current_row = [1] * n

        for _ in range(1, m):
            for col in range(1, n):
                current_row[col] += current_row[col - 1]

        return current_row[-1]
```

### 💡 Example: m = 3, n = 3

* Start: `[1, 1, 1]`
* After row 1: `[1, 2, 3]`
* After row 2: `[1, 3, 6]`

✅ Final result: `6`

---

### ⏱ Time and Space Complexity

* **Time:** O(m × n)
* **Space:** O(n)

---

## 🧑‍💼 Interviewer Expectations

| 💬 Topic         | ✅ What to Say                                                      |
| ---------------- | ------------------------------------------------------------------ |
| Problem Type     | Classic **2D Dynamic Programming**                                 |
| State Definition | `dp[i][j]` = number of ways to reach cell `(i, j)`                 |
| Base Case        | First row & column = `1` (only one way to reach)                   |
| Transition       | `dp[i][j] = dp[i-1][j] + dp[i][j-1]`                               |
| Optimization     | Space can be reduced to O(n)                                       |
| Alternatives     | You can also solve using **combinatorics**: `(m+n-2) choose (m-1)` |

---

## 📚 Bonus: Combinatorics Formula

If you're asked for a **mathematical approach**:

```
Number of paths = C(m + n - 2, m - 1) = (m + n - 2)! / [(m - 1)! * (n - 1)!]
```

✅ O(1) space, fast — but risk of overflow for large values.

---

## 📌 Summary

* ✅ Use bottom-up DP for grid movement problems.
* ✅ Always consider **space optimizations** if previous rows are no longer needed.
* ✅ Be ready to explain both **DP intuition** and **combinatorics** in interviews.
* ✅ Common pattern for questions involving movement in a grid with constraints.

---
