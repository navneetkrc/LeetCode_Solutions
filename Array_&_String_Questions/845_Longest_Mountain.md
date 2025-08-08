# 📘 Leetcode 845 - Longest Mountain in Array

---

## 🧩 Problem Descriptions

Given an integer array `arr`, return the length of the longest mountain.

A **mountain** is defined as:

* `arr.length >= 3`
* There exists some index `i` such that:

  * `arr[0] < arr[1] < ... < arr[i]` (**strictly increasing** up to the peak)
  * `arr[i] > arr[i+1] > ... > arr[n-1]` (**strictly decreasing** after the peak)

### 🔍 Examples

#### Example 1

```
Input: arr = [2,1,4,7,3,2,5]
Output: 5
Explanation: The mountain is [1,4,7,3,2]
```

#### Example 2

```
Input: arr = [2,2,2]
Output: 0
Explanation: No mountain exists.
```

---

## 🎯 Constraints

* `1 <= arr.length <= 10^4`
* `0 <= arr[i] <= 10^4`

---

## ✅ What Interviewers Expect

* Clear understanding of **mountain definition** (increasing then decreasing).
* Ability to optimize space/time: ideally to **O(n)** time and **O(1)** space.
* Clear explanation of edge cases (flat parts, small arrays, single slopes).
* Knowing when to use **two-pass DP**, **brute force**, or **greedy one-pass**.

---

## ✅ Final Interview-Ready Code (💡 One-Pass, O(1) Space)

```python
from typing import List

class Solution:
    def longestMountain(self, arr: List[int]) -> int:
        n = len(arr)
        longest_mountain = 0
        current_index = 1  # Start from the second element

        while current_index < n - 1:
            is_peak = arr[current_index - 1] < arr[current_index] > arr[current_index + 1]

            if is_peak:
                left_index = current_index - 1
                right_index = current_index + 1

                # Expand leftwards while increasing
                while left_index > 0 and arr[left_index - 1] < arr[left_index]:
                    left_index -= 1

                # Expand rightwards while decreasing
                while right_index < n - 1 and arr[right_index] > arr[right_index + 1]:
                    right_index += 1

                # Update max mountain length
                mountain_length = right_index - left_index + 1
                longest_mountain = max(longest_mountain, mountain_length)

                # Jump to end of current mountain
                current_index = right_index
            else:
                current_index += 1

        return longest_mountain
```

---

## 🔍 Visual Intuition

```
                ▲
              ▲   ▲
   Start →  ▲       ▼       ← End
         ▲            ▼
      ▲                 ▼
   1   4   7   3   2

↑ Left scan expands while increasing  
↓ Right scan expands while decreasing
```

---

## 🧠 Interview Talk-Through Tips

When asked to solve:

1. 🔹 Start with **brute force** idea (check all subarrays) — O(n²).
2. 🔹 Suggest **2-pass solution** with auxiliary arrays (`up[]` and `down[]`) — O(n) time and space.
3. 🔹 Then **optimize to 1-pass** with O(1) space — interviewers love this.
4. ⚠️ Call out edge cases:

   * Flat arrays: `[2,2,2]`
   * Only increasing/decreasing: `[1,2,3,4]`, `[4,3,2,1]`
   * Less than 3 elements

---

## 🔁 Alternate Approach (🧱 Two Pass using up/down arrays)

```python
class Solution:
    def longestMountain(self, arr: List[int]) -> int:
        n = len(arr)
        up = [0] * n
        down = [0] * n

        # Count increasing sequence from left
        for i in range(1, n):
            if arr[i] > arr[i - 1]:
                up[i] = up[i - 1] + 1

        # Count decreasing sequence from right
        for i in range(n - 2, -1, -1):
            if arr[i] > arr[i + 1]:
                down[i] = down[i + 1] + 1

        longest_mountain = 0
        for i in range(1, n - 1):
            if up[i] > 0 and down[i] > 0:
                mountain_length = up[i] + down[i] + 1
                longest_mountain = max(longest_mountain, mountain_length)

        return longest_mountain
```

---

## 📌 Summary Table

| Approach             | Time Complexity | Space Complexity | Notes                  |
| -------------------- | --------------- | ---------------- | ---------------------- |
| Brute Force          | O(n²)           | O(1)             | Try all subarrays      |
| Two-Pass (up/down)   | O(n)            | O(n)             | Easier to implement    |
| **One-Pass Optimal** | **O(n)**        | **O(1)**         | 🔥 Best for interviews |

---

## 📦 Sample Test Cases

```python
s = Solution()
assert s.longestMountain([2,1,4,7,3,2,5]) == 5
assert s.longestMountain([2,2,2]) == 0
assert s.longestMountain([0,1,2,3,4,5,4,3,2,1,0]) == 11
assert s.longestMountain([1,3,2]) == 3
```

---
