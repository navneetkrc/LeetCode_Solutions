# 🧗‍♂️ Leetcode 70: Climbing Stairs
---

## 📘 Problem Description

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either **climb 1 or 2 steps**. In how many distinct ways can you climb to the top?

### 🧠 Example 1:
```

Input: n = 2
Output: 2
Explanation: 1 step + 1 step, or 2 steps

```

### 🧠 Example 2:
```

Input: n = 3
Output: 3
Explanation: 1+1+1, 1+2, 2+1

```

---

## 🛠️ Approach 1: Bottom-Up Dynamic Programming (Tabulation)

This is a variation of the **Fibonacci sequences**, where:

```

ways(n) = ways(n-1) + ways(n-2)

````

Why?  
- To reach step `n`, you either came from `n-1` (1 step up) or `n-2` (2 steps up).  
- So total ways to reach `n` = ways to reach `n-1` + ways to reach `n-2`

---

## ✅ Clean Code with Descriptive Variables

```python
class Solution:
    def climbStairs(self, total_steps: int) -> int:
        # Edge cases for n = 1 or 2
        if total_steps == 1:
            return 1
        if total_steps == 2:
            return 2

        # Initialize DP table with base cases
        ways_to_climb = [0] * (total_steps + 1)
        ways_to_climb[1] = 1  # 1 way to climb 1 step
        ways_to_climb[2] = 2  # 2 ways to climb 2 steps

        # Build solution from bottom-up
        for step in range(3, total_steps + 1):
            ways_to_climb[step] = ways_to_climb[step - 1] + ways_to_climb[step - 2]

        return ways_to_climb[total_steps]
````

---

## 🧠 Interview Talking Points

| 💡 Topic            | 💬 What to Say                                                          |
| ------------------- | ----------------------------------------------------------------------- |
| 🎯 Problem Type     | It's a **Dynamic Programming** problem similar to **Fibonacci numbers** |
| 🔁 State Definition | `dp[i]` = Number of ways to reach step `i`                              |
| 🧱 Base Cases       | `dp[1]=1`, `dp[2]=2` — 1 or 2 ways to reach those steps                 |
| 🔗 Transition       | From step `i`, you can reach from `i-1` or `i-2`                        |
| ⏱ Time Complexity   | O(n)                                                                    |
| 📦 Space Complexity | O(n) — can be reduced to O(1)                                           |

---

## 🧠 Dry Run

### For `n = 5`:

```
ways[1] = 1  
ways[2] = 2  
ways[3] = 3 (1+2 or 2+1)  
ways[4] = 5  
ways[5] = 8
```

Final Answer: 8 distinct ways to reach step 5 ✅

---

## 🛠️ Approach 2: Space Optimized Fibonacci

Only store the last two results instead of a full array.

```python
class Solution:
    def climbStairs(self, total_steps: int) -> int:
        if total_steps == 1:
            return 1
        first, second = 1, 2
        for _ in range(3, total_steps + 1):
            first, second = second, first + second
        return second
```

* ✅ Time: O(n)
* ✅ Space: O(1)

Use this when asked to **optimize for space**.

---

## 🛠️ Approach 3: Precomputed Array (Constant Time Lookup)

If you expect many repeated calls, precompute all answers once.

```python
class Solution:
    # Precompute values for n up to 49
    step_ways = [0] * 50
    step_ways[1] = 1
    step_ways[2] = 2
    for i in range(3, 50):
        step_ways[i] = step_ways[i - 1] + step_ways[i - 2]

    def climbStairs(self, total_steps: int) -> int:
        return self.step_ways[total_steps]
```

* ⚡ Fastest runtime, great when function is called many times
* But **inflexible** — hardcoded limit (can be extended)

---

## 🧪 Common Follow-Ups

| ❓ Question                                 | ✅ Talking Point                    |
| ------------------------------------------ | ---------------------------------- |
| What if steps can be 1, 2, or 3?           | Add `dp[i-3]` to transition        |
| What if steps have costs?                  | Use **Minimum Path Sum** variation |
| Can you count paths with exact step usage? | DP with additional tracking        |

---

## 📌 Summary

* ✅ Use **DP** when decisions depend on previous results.
* ✅ Reduce space when only last two states are needed.
* ✅ Precompute only if many queries are expected.
* ✅ Practice dry-runs and edge cases (`n = 0`, `n = 1`, `n = 2`).

---
