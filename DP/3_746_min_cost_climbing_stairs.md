# 🪜 Leetcode 746: Min Cost Climbing Stairs

---

## 📘 Problem Description

You are given an integer array `cost` where `cost[i]` is the cost of stepping on the `i-th` stair.

You can start at either step `0` or step `1`.  
At each move, you can climb either **1 or 2 steps**.  
Your goal is to **reach the top (just after the last stair)** in the **minimum total cost**.

---

### 🧠 Example 1:

```

Input: cost = [10, 15, 20]
Output: 15
Explanation: Start at index 1 (cost = 15), then climb 2 steps to the top.

```

### 🧠 Example 2:

```

Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Take optimal steps to avoid expensive stairs.

```

---

## 💡 Intuition

To reach the **top (after last index)**, we can either:
- Climb 1 step from the second last stair  
- Or climb 2 steps from the third last stair  

So for every position `i`, we compute:
```

dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])

````

The minimum cost to reach the top will be `dp[n]`.

---

## ✅ Approach 1: Clean and Simple DP

### 🧠 Idea:
- Build the answer **bottom-up** using a DP array.
- Each `dp[i]` stores the **minimum cost** to reach step `i`.

```python
from typing import List

class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [0] * (n + 1)  # dp[i] = min cost to reach step i

        for i in range(2, n + 1):
            cost_from_one_step = dp[i - 1] + cost[i - 1]
            cost_from_two_steps = dp[i - 2] + cost[i - 2]
            dp[i] = min(cost_from_one_step, cost_from_two_steps)

        return dp[n]
````

### ⏱ Complexity:

* **Time:** O(n)
* **Space:** O(n)

---

## ✅ Approach 2: Space Optimized DP

### 🧠 Idea:

* Since we only need the **last two states**, we don’t need a full array.
* Use variables to simulate the `dp` array.

```python
from typing import List

class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        prev_step_cost = 0  # dp[i-1]
        prev_prev_step_cost = 0  # dp[i-2]

        for i in range(2, n + 1):
            jump_one = prev_step_cost + cost[i - 1]
            jump_two = prev_prev_step_cost + cost[i - 2]
            current_cost = min(jump_one, jump_two)

            # Shift the window
            prev_prev_step_cost = prev_step_cost
            prev_step_cost = current_cost

        return prev_step_cost
```

### ⏱ Complexity:

* **Time:** O(n)
* **Space:** O(1)

---

## 🔍 Dry Run for `[10, 15, 20]`

| Step `i` | dp[i-1] | dp[i-2] | cost[i-1] | cost[i-2] | dp[i] = min(...) |
| -------- | -------- | -------- | ---------- | ---------- | ----------------- |
| 2        | 0        | 0        | 15         | 10         | min(15, 10) = 10  |
| 3        | 10       | 0        | 20         | 15         | min(30, 15) = 15  |

✅ Final Answer: `15`

---

## 🧑‍💼 What to Say in Interviews

| 🧠 Concept       | 💬 Explanation                                                        |
| ---------------- | --------------------------------------------------------------------- |
| Problem Type     | This is a classic **DP over stairs** problem.                         |
| State Definition | `dp[i]` = min cost to reach step `i`                                  |
| Recurrence       | `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`               |
| Initialization   | `dp[0] = 0`, `dp[1] = 0` — since you can start at either step         |
| Edge Cases       | Arrays with 1 or 2 elements                                           |
| Optimization     | Space can be reduced from O(n) to O(1) by tracking only last 2 values |

---

## 📌 Summary

* Use bottom-up DP to model step choices.
* Optimize to O(1) space by tracking only two variables.
* Focus on explaining how the **minimum cost accumulates** step by step.
* A foundational problem for ladder/jump/step-based DP patterns.

---
