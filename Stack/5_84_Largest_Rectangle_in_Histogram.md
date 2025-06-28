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

## 🧾 Python Code with Comments

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
