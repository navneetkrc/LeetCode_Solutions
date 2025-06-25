# 🪨 1046. Last Stone Weight – Complete Interview Guide

**Difficulty:** Easy
**Topic Tags:** Heap, Priority Queue, Simulation
**Asked by:** Amazon, Google, Bloomberg

---

## 🧩 Problem Statement

You are given an array `stones` where `stones[i]` is the weight of the `i-th` stone.

### 🕹️ Game Rules:

* Pick the **two heaviest** stones: `x` and `y` (`x ≤ y`)
* If `x == y`: both are destroyed
* If `x != y`: destroy `x`, and replace `y` with `y - x`

🔚 Repeat until one or zero stones remain.

### 🎯 Goal:

Return the **weight of the last remaining stone**, or `0` if none are left.

---

## 🧪 Examples

### 🔸 Example 1

```txt
Input:  stones = [2,7,4,1,8,1]
Output: 1
```

**Steps:**

```
[2,7,4,1,8,1] 
→ smash 8 & 7 → insert 1 → [2,4,1,1,1]
→ smash 4 & 2 → insert 2 → [2,1,1,1]
→ smash 2 & 1 → insert 1 → [1,1,1]
→ smash 1 & 1 → insert 0 → [1]
→ Final answer = 1
```

---

### 🔸 Example 2

```txt
Input:  stones = [1]
Output: 1
```

---

## ✅ Constraints

* `1 <= stones.length <= 30`
* `1 <= stones[i] <= 1000`

---

## 🎯 Key Observations

| Insight 🔍                     | Why it matters 💡                                  |
| ------------------------------ | -------------------------------------------------- |
| Need top 2 elements repeatedly | Use a **max-heap** for fast retrieval              |
| Python has only min-heap       | Simulate max-heap by **negating values**           |
| Sort-based approach is slower  | Inefficient due to repeated sorting (`O(n log n)`) |

---

# 🚀 Optimal Approach – Max Heap (Using `heapq`)

### 🛠️ Strategy

* Convert all stone weights to **negatives**.
* Use Python's `heapq` (a min-heap) to simulate a **max-heap**.
* Repeatedly **pop two largest stones**, compute the difference, and push it back (if non-zero).

---

## ✅ Clean Python Code – Heap-Based

```python
from typing import List
import heapq

class Solution:
    def lastStoneWeight(self, stone_weights: List[int]) -> int:
        """
        Simulates the stone smashing process using a max-heap.
        Python’s heapq is a min-heap, so we invert values for max-heap behavior.
        """
        # Negate the stone weights for max-heap simulation
        stone_weights = [-w for w in stone_weights]
        heapq.heapify(stone_weights)

        while len(stone_weights) > 1:
            heaviest = -heapq.heappop(stone_weights)
            second_heaviest = -heapq.heappop(stone_weights)

            if heaviest != second_heaviest:
                diff = heaviest - second_heaviest
                heapq.heappush(stone_weights, -diff)

        return -stone_weights[0] if stone_weights else 0
```

---

## 📊 Visual Walkthrough – Heap View

For input `[2,7,4,1,8,1]`:

```
Heapified (negated): [-8, -7, -4, -2, -1, -1]

Step 1: pop 8 & 7 → push -1      → [-4, -2, -1, -1, -1]
Step 2: pop 4 & 2 → push -2      → [-2, -1, -1, -1]
Step 3: pop 2 & 1 → push -1      → [-1, -1, -1]
Step 4: pop 1 & 1 → cancel out   → [-1]
```

✅ Remaining stone = **1**

---

## ⏱️ Time & Space Complexity

| Complexity Metric | Value        |
| ----------------- | ------------ |
| Time              | `O(n log n)` |
| Space             | `O(n)`       |

---

## 👩‍💻 What Interviewers Expect

| Expectation                  | Explanation / What to Say                               |
| ---------------------------- | ------------------------------------------------------- |
| **Problem understanding**    | Explain how the smash operation works (equal ≠ unequal) |
| **Optimal structure choice** | Suggest heap for efficient top-2 selection              |
| **Python trick explanation** | Clarify why negation simulates a max-heap               |
| **Edge cases**               | Handle 1 stone, all same weight, all different          |
| **Readable code**            | Use descriptive names for clarity                       |

---

# 🧪 Alternate Approach – Sort-Based Simulation

> Use this if asked *not* to use a heap.

---

### 💡 Logic

* Sort the stones in descending order
* Smash top 2 stones, insert back the result
* Repeat until ≤1 stone remains

---

## 🔧 Python Code – Sort Version

```python
class Solution:
    def lastStoneWeight_sort_simulation(self, stone_weights: List[int]) -> int:
        """
        Brute-force simulation using repeated sorting.
        Less efficient than a heap.
        """
        while len(stone_weights) > 1:
            stone_weights.sort()
            heaviest = stone_weights.pop()
            second_heaviest = stone_weights.pop()

            if heaviest != second_heaviest:
                stone_weights.append(heaviest - second_heaviest)

        return stone_weights[0] if stone_weights else 0
```

---

## ⏱️ Time & Space Complexity

| Metric | Value                      |
| ------ | -------------------------- |
| Time   | `O(n² log n)` (worst case) |
| Space  | `O(1)` (in-place sorting)  |

---

## ⚠️ When to Use This Approach

* Suitable **only** for small inputs (`n ≤ 30`)
* Good for explaining alternatives in interviews
* Demonstrates willingness to explore multiple ideas

---

## 🧠 Interview Communication Tip

> “While sorting works for this small input size, I prefer using a max-heap for performance and scalability. Python’s `heapq` supports only min-heap, so I used negative values to simulate max behavior.”

---

# 🏁 Summary & Comparison

| Approach        | Efficiency | Notes                                        |
| --------------- | ---------- | -------------------------------------------- |
| **Heap (main)** | ✅ Optimal  | Clean, fast, standard – ideal for interviews |
| **Sort (alt)**  | ❌ Slower   | Simple, but inefficient for large inputs     |

---

## 📚 Final Takeaways for Interviews

✅ Emphasize mapping problem rules to the **max-heap structure**

✅ Clearly explain Python's **negation trick** for heap usage

✅ Discuss **edge cases**: 1 stone, all equal, no stones

✅ Mention **alternate strategy** (sort-based) when asked

✅ Show understanding of **time/space trade-offs**

---
