# 🧭 Leetcode 1266: Minimum Time Visiting All Points

---

## 📘 Problem Description

You're given an array `points` where `points[i] = [xi, yi]` represents a point on the 2D plane.

You need to **visit all the given points in the order** they appear.  
You can move:
- 🟩 **Horizontally** (change in `x`)
- 🟦 **Vertically** (change in `y`)
- 🔶 **Diagonally** (change in both `x` and `y` by 1)

Each move takes **1 unit of time**.

> Return the **minimum time** needed to visit all points.

---

### 🧪 Example

```text
Input: points = [[1,1],[3,4],[-1,0]]
Output: 7

Explanation:
(1,1) → (2,2) → (3,3) → (3,4)  → (2,3) → (1,2) → (0,1) → (-1,0)
Steps: 3 (first jump) + 4 (second jump) = 7
````

---

## 🧠 Interview Intent

This problem tests:

* Understanding of geometry and movements
* Optimal movement strategy (diagonal vs straight)
* Clean looping through point pairs
* **Explaining a greedy move intuition**
* **Use of `max(dx, dy)` rather than brute-force simulation**

---

## 🧩 Key Observation

To move from point A `(x1, y1)` to point B `(x2, y2)`:

* You can cover **both x and y simultaneously** via diagonal moves.
* The most efficient path is:

  ```
  min(dx, dy) diagonal moves
  + remaining horizontal or vertical moves
  = max(dx, dy)
  ```

---

## ✅ Final Approach using Enumerate (Greedy + Intuition)

```python
from typing import List

class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        total_time = 0

        # Start from second point and compare with previous
        for i, (curr_x, curr_y) in enumerate(points[1:], start=1):
            prev_x, prev_y = points[i - 1]

            # Horizontal and vertical distances
            delta_x = abs(curr_x - prev_x)
            delta_y = abs(curr_y - prev_y)

            # Minimum time is governed by the longer distance
            time_to_next = max(delta_x, delta_y)

            total_time += time_to_next

        return total_time
```
---

## ✅ Final Approach using 🔸 Index-Based Pairwise Traversal (Greedy + Intuition)

```python
from typing import List

class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        total_time = 0

        for i in range(1, len(points)):
            # Extract previous and current point
            x1, y1 = points[i - 1]
            x2, y2 = points[i]
            
            # Calculate horizontal and vertical distances
            dx = abs(x2 - x1)
            dy = abs(y2 - y1)

            # Since diagonal moves are allowed, we can take max(dx, dy) steps
            total_time += max(dx, dy)

        return total_time
---
```

## 🧮 Dry Run on `[[1,1], [3,4], [-1,0]]`

| From → To      | `dx` | `dy` | `max(dx, dy)` |
| -------------- | ---- | ---- | ------------- |
| (1,1) → (3,4)  | 2    | 3    | 3             |
| (3,4) → (-1,0) | 4    | 4    | 4             |
| ✅ Total Time   |      |      | **7**         |

---

## 📌 Edge Cases

| Case                | Output |
| ------------------- | ------ |
| Only one point      | 0      |
| Repeated points     | 0      |
| Straight horizontal | dx     |
| Straight vertical   | dy     |
| Fully diagonal      | dx==dy |

---

## ✍️ What to Explain in the Interview

> “Since diagonal moves allow simultaneous movement in both x and y directions, the most optimal step from one point to another takes `max(|x2 - x1|, |y2 - y1|)` moves. We loop through consecutive pairs and accumulate that.”

You can optionally **visualize** like:

```
(1,1) → (2,2) → (3,3) → (3,4)
(3,4) → (2,3) → (1,2) → (0,1) → (-1,0)
```

---

## 🧪 Test Cases

```python
s = Solution()

assert s.minTimeToVisitAllPoints([[1,1],[3,4],[-1,0]]) == 7
assert s.minTimeToVisitAllPoints([[3,2],[3,2]]) == 0
assert s.minTimeToVisitAllPoints([[0,0],[1,1],[2,2]]) == 2
assert s.minTimeToVisitAllPoints([[0,0],[2,3]]) == 3
assert s.minTimeToVisitAllPoints([[0,0],[0,5]]) == 5
```

---

## 🧾 Summary Table for Movement Strategy

| Strategy        | Moves         | Time        |
| --------------- | ------------- | ----------- |
| Diagonal move   | x+1, y+1      | 1 unit      |
| Horizontal move | x+1, y        | 1 unit      |
| Vertical move   | x, y+1        | 1 unit      |
| Optimal Steps   | `max(dx, dy)` | ✅ Best Time |

---

## 🧑‍💼 Wrap-up Tips

* Use clean variable names: `prev_x`, `curr_x`, `delta_x`, etc.
* **Explain greedy + math insight: “Since we can reduce both dimensions with diagonal steps, we take the max.”**
* Discuss edge cases.
* Bonus: Show a few moves visually.

---

