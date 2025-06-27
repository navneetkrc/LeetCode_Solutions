# 🌡️ 739. Daily Temperatures – Complete Interview Guide

**Difficulty:** Medium  
**Tags:** Stack, Monotonic Stack, Array  
**Asked By:** Amazon, Facebook, Microsoft, Google

---

## 📘 Problem Statement

Given an array `temperatures` representing daily temperatures, return an array `answer` such that:

- `answer[i]` = number of days to wait **after day i** to get a **warmer temperature**.
- If there is **no future day** with a warmer temperature, set `answer[i] = 0`.

---

## 🔢 Examples

### Example 1
```text
Input:  temperatures = [73, 74, 75, 71, 69, 72, 76, 73]
Output: [1, 1, 4, 2, 1, 1, 0, 0]
````

### Example 2

```text
Input:  temperatures = [30, 40, 50, 60]
Output: [1, 1, 1, 0]
```

### Example 3

```text
Input:  temperatures = [30, 60, 90]
Output: [1, 1, 0]
```

---

## 💡 Key Observations

* You need to **track previous colder days** and check if a **warmer day appears** later.
* You only care about the **next warmer day**, not the actual temperature difference.
* A **monotonic decreasing stack** (descending temperatures) helps track unresolved days.

---

## ✅ What Interviewers Expect

* Efficient algorithm (not brute-force O(n²)).
* Correct use of **stack** to track unresolved indices.
* Clear explanation of:

  * What the stack holds (temperature, index)
  * When to pop from the stack
  * How the answer is computed.

---

## 👨‍💻 Python Code (Clean + Self-Explanatory)

```python
from typing import List

class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        total_days = len(temperatures)
        answer = [0] * total_days  # Default: 0 (no warmer day)
        stack = []  # Stack stores pairs: (temperature, index)

        # Traverse temperatures from left to right
        for current_day, current_temp in enumerate(temperatures):
            # Check if current temperature is warmer than temperature at top of the stack
            while stack and current_temp > stack[-1][0]:
                prev_temp, prev_day = stack.pop()
                answer[prev_day] = current_day - prev_day  # Days waited for warmer temp

            # Push current temperature with its index onto the stack
            stack.append((current_temp, current_day))

        return answer
```

---
## 👨‍💻 Python Code (Clean + Self-Explanatory)

```python

from typing import List

class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        total_days = len(temperatures)
        result = [0] * total_days  # Initialize result with all 0s
        stack = []  # Stack stores indices of unresolved days

        # Traverse the temperatures list from left to right
        for current_day_index, current_day_temp in enumerate(temperatures):
            
            # Resolve all previous days where current day's temperature is warmer
            while stack and temperatures[stack[-1]] < current_day_temp:
                previous_day_index = stack.pop()
                days_to_wait = current_day_index - previous_day_index
                result[previous_day_index] = days_to_wait

            # Push current index onto the stack for future comparison
            stack.append(current_day_index)

        return result
```

---



---

## 🔍 Step-by-Step Dry Run

### Input: `[73, 74, 75, 71, 69, 72, 76, 73]`

| Day | Temp | Stack State (Top → Bottom)   | Action                         | Answer             |
| --- | ---- | ---------------------------- | ------------------------------ | ------------------ |
| 0   | 73   | \[(73, 0)]                   | Push (73, 0)                   | \[0,0,0,0,0,0,0,0] |
| 1   | 74   | \[]                          | 74 > 73 → Pop → answer\[0] = 1 | \[1,0,0,0,0,0,0,0] |
|     |      | \[(74, 1)]                   | Push (74, 1)                   |                    |
| 2   | 75   | \[]                          | 75 > 74 → Pop → answer\[1] = 1 | \[1,1,0,0,0,0,0,0] |
|     |      | \[(75, 2)]                   | Push (75, 2)                   |                    |
| 3   | 71   | \[(75, 2), (71, 3)]          | Push (71, 3)                   |                    |
| 4   | 69   | \[(75, 2), (71, 3), (69, 4)] | Push (69, 4)                   |                    |
| 5   | 72   | \[(75, 2), (71, 3)]          | 72 > 69 → Pop → answer\[4] = 1 | \[1,1,0,0,1,0,0,0] |
|     |      |                              | 72 > 71 → Pop → answer\[3] = 2 | \[1,1,0,2,1,0,0,0] |
|     |      | \[(75, 2), (72, 5)]          | Push (72, 5)                   |                    |
| 6   | 76   | \[]                          | 76 > 72 → Pop → answer\[5] = 1 | \[1,1,0,2,1,1,0,0] |
|     |      |                              | 76 > 75 → Pop → answer\[2] = 4 | \[1,1,4,2,1,1,0,0] |
|     |      | \[(76, 6)]                   | Push (76, 6)                   |                    |
| 7   | 73   | \[(76, 6), (73, 7)]          | Push (73, 7)                   |                    |

✅ Final Answer: `[1, 1, 4, 2, 1, 1, 0, 0]`

---

## ⏱ Time and Space Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | O(n)       |
| Space | O(n)       |

* Every index is pushed and popped from the stack **at most once**.
* Stack grows in the worst case to `O(n)`.

---

## 🧠 What to Say During Interviews

> “This is a classic **Next Greater Element** problem, adapted to temperatures.”

✅ “I maintain a **monotonic decreasing stack** of `(temperature, index)`.”
✅ “When I find a higher temperature, I resolve all previous colder days in the stack.”
✅ “The difference in indices gives the number of days to wait.”
✅ “Each element is processed once — so it's efficient and linear.”

---

## 🔄 Related Problems for Practice

* **496. Next Greater Element I**
* **503. Next Greater Element II (circular version)**
* **84. Largest Rectangle in Histogram**
* **901. Online Stock Span**

---

## 📌 Interview Tip

This problem demonstrates:

* 🧠 Stack design patterns
* ⚡️ Time-efficient traversal logic
* 🧰 Using tuples in stack for index tracking

---

**🔥 Mastering this ensures you're prepared for a class of stack-based interview questions!**

