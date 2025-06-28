# 📊 84. Largest Rectangle in Histogram – Interview-Ready Guide

**Difficulty:** Hard
**Tags:** Stack, Monotonic Stack, Array
**Companies:** Amazon, Google, Facebook, Microsoft, Bloomberg

---

## 🧩 Problem Statement

You are given an array of integers `heights` representing the heights of bars in a histogram. The width of each bar is `1`.

Your task is to find the **area of the largest rectangle** that can be formed within the bounds of the histogram.

---

## 📥 Example

```python
Input: heights = [2,1,5,6,2,3]
Output: 10
```

**Explanation:** The largest rectangle has height = 2 and spans from index 2 to 3 (i.e., values 5 and 6).
Area = 2 (height) × 5 (width) = **10**

---

## 💡 Intuition – Expand While Minimum

For each bar `i`:

* Treat it as the **shortest bar** in the rectangle.
* Move **left** and **right** as long as the bars are `≥ heights[i]`.

### Formula:

```
area = (count_left + count_right + 1) * heights[i]
```

---

## ✅ Algorithm Steps

1. **Traverse** each index `i` of the array.
2. For each index:

   * Count how many bars to the **left** are ≥ `heights[i]`
   * Count how many bars to the **right** are ≥ `heights[i]`
3. Compute the area using the formula.
4. Track the **maximum area** found.

---

## 🧾 Brute-Force Python Code with Comments

```python
from typing import List

class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        max_area = 0
        n = len(heights)

        for i in range(n):
            current_height = heights[i]

            # 🔼 Count bars to the left >= current_height
            count_left = 0
            j = i - 1
            while j >= 0 and heights[j] >= current_height:
                count_left += 1
                j -= 1

            # 🔽 Count bars to the right >= current_height
            count_right = 0
            j = i + 1
            while j < n and heights[j] >= current_height:
                count_right += 1
                j += 1

            # 📦 Compute area for current bar as the smallest
            width = count_left + count_right + 1
            area = current_height * width
            max_area = max(max_area, area)

        return max_area
```

---

## 🧪 Dry Run Example

**Input:** `[2,1,5,6,2,3]`

Let’s take `heights[2] = 5`:

* Count left: index 1 → 1 < 5 → stop → `count_left = 0`
* Count right: index 3 → 6 ≥ 5 → continue → index 4 → 2 < 5 → stop → `count_right = 1`

→ Area = `(0 + 1 + 1) * 5 = 10`

---

## ⏱️ Complexity

| Type  | Value |
| ----- | ----- |
| Time  | O(n²) |
| Space | O(1)  |

> This is **not optimal** for large inputs. Use this approach when clearly asked in interviews to use “expand from minimum bar” logic.

---

## 💬 Interview Strategy

### ✅ Talk About:

* Assumptions: Width of each bar is 1.
* Brute-force vs optimized approaches.
* Importance of local expansion when bar is treated as min.

### 🚀 Suggest Improvements:

* Offer to use Monotonic Stack to achieve **O(n)** time if interviewer asks for optimization.

---

## 🏁 Summary

This approach is:

* Intuitive and interview-friendly.
* Good for demonstrating expansion logic and area computation.
* A stepping stone to optimized solutions.

---

## ⚙️ Optimized Approach: Monotonic Stack (O(n) Time)

The **Monotonic Stack** approach allows us to efficiently determine the **next smaller element to the left and right** for every bar.

### 🔧 Strategy:

* Append a `0` at the end to flush the stack.
* Maintain a stack with indices where the heights are **in increasing order**.
* When a bar breaks this order, pop from the stack and compute area.

### ✅ Python Code:

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []  # stack to store indices
        max_area = 0
        heights.append(0)  # add sentinel bar

        for i, h in enumerate(heights):
            while stack and heights[stack[-1]] > h:
                height = heights[stack.pop()]
                # If stack is empty, width = i
                width = i if not stack else i - stack[-1] - 1
                max_area = max(max_area, height * width)
            stack.append(i)

        return max_area
```
---
### ✅ Python Code:

```python
from typing import List

class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        num_bars = len(heights)
        index_stack = []  # Stack to store (bar_height, start_index)
        max_rectangle_area = 0

        for current_index, current_height in enumerate(heights):
            start_index = current_index

            # Pop from stack while current bar is smaller than the top of the stack
            while index_stack and current_height < index_stack[-1][0]:
                previous_height, previous_index = index_stack.pop()
                width = current_index - previous_index
                area = previous_height * width
                max_rectangle_area = max(max_rectangle_area, area)
                start_index = previous_index  # Expand left boundary

            # Push current bar with its effective start index
            index_stack.append((current_height, start_index))

        # Process remaining bars in the stack
        while index_stack:
            height, start_index = index_stack.pop()
            width = num_bars - start_index
            area = height * width
            max_rectangle_area = max(max_rectangle_area, area)

        return max_rectangle_area
```


### ⏱️ Complexity

| Type  | Value |
| ----- | ----- |
| Time  | O(n)  |
| Space | O(n)  |

---

## ✅ When to Use Which

| Method          | Time  | Use Case                                   |
| --------------- | ----- | ------------------------------------------ |
| Expand from Min | O(n²) | Small input or when asked to brute-force   |
| Monotonic Stack | O(n)  | Optimal for large inputs or production use |

---

# 🧠 Monotonic Stack Visual Walkthrough – Largest Rectangle in Histogram

Let's walk through the **Monotonic Stack** approach to solve **Leetcode 84** with a **step-by-step visual trace** using the input:

```python
heights = [2, 1, 5, 6, 2, 3]
```

We'll **append a 0** at the end to flush out remaining elements:

```python
heights = [2, 1, 5, 6, 2, 3, 0]
```

---

## 🔄 Initialization

* `stack = []`  → stores **indices**
* `max_area = 0`

---

## 🎞️ Animated-Style Side-by-Side Trace

Each frame shows the current state as we process the array.

| Step | Index `i` | Height `h` | Stack Before | Action Taken                               | Stack After | max\_area |
| ---- | --------- | ---------- | ------------ | ------------------------------------------ | ----------- | --------- |
| 1    | 0         | 2          | \[]          | push (0)                                   | \[0]        | 0         |
| 2    | 1         | 1          | \[0]         | pop (0 → height 2, width 1 = 2)            | \[1]        | 2         |
| 3    | 2         | 5          | \[1]         | push (2)                                   | \[1, 2]     | 2         |
| 4    | 3         | 6          | \[1, 2]      | push (3)                                   | \[1, 2, 3]  | 2         |
| 5    | 4         | 2          | \[1, 2, 3]   | pop (3 → 6×1=6), pop (2 → 5×2=10), push(4) | \[1, 4]     | 10        |
| 6    | 5         | 3          | \[1, 4]      | push (5)                                   | \[1, 4, 5]  | 10        |
| 7    | 6         | 0          | \[1, 4, 5]   | pop (5 → 3×1=3), pop (4 → 2×4=8),          | \[6]        | 10        |
|      |           |            | \[1]         | pop (1 → 1×6=6), push (6)                  | \[6]        | 10        |

---

## ✅ Final Result

* **Max Area = 10**

---

## 🧠 Interpretation

* The stack helps maintain an **increasing height sequence**.
* When a smaller height is found, we know it's the right boundary for prior taller bars.
* We compute the area for each popped bar using its height and the width between current and previous indices.

---

