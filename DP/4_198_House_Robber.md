# 🏠 Leetcode 198: House Robber

---

## 📘 Problem Description

You are a professional robber planning to rob houses along a street.  
Each house has a certain amount of money stashed, and the only constraint is that **you cannot rob two adjacent houses**.

Given an integer array `nums` representing the amount of money of each house, return the **maximum amount** of money you can rob **without alerting the police**.

---

### 🧠 Example 1:

```

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (value = 1) and house 3 (value = 3) → Total = 1 + 3 = 4

```

### 🧠 Example 2:

```

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (2), house 3 (9), house 5 (1) → Total = 2 + 9 + 1 = 12

````

---

## 🔍 Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

---

## 💡 Intuition

This is a classic **dynamic programming** problem with a simple rule:
> If you rob a house, you can't rob the next one.

At each house `i`, you have two choices:
1. **Rob it** → you add `nums[i]` to the amount from house `i-2`
2. **Skip it** → you take the max amount from house `i-1`

---

## ✅ Approach 1: DP with Rob/Skip Arrays (Your Code, Cleaned & Explained)

```python
from typing import List

class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        
        # Handle edge cases
        if n == 0:
            return 0
        if n == 1:
            return nums[0]
        
        # dp_rob[i] = max amount if we rob house i
        # dp_skip[i] = max amount if we skip house i
        dp_rob = [0] * n
        dp_skip = [0] * n

        dp_rob[0] = nums[0]
        dp_skip[0] = 0

        for i in range(1, n):
            dp_rob[i] = dp_skip[i - 1] + nums[i]
            dp_skip[i] = max(dp_rob[i - 1], dp_skip[i - 1])

        # Return the best of robbing or skipping the last house
        return max(dp_rob[-1], dp_skip[-1])
````

---

## 🔍 Dry Run Example: `nums = [2,7,9,3,1]`

| House | Rob (dp\_rob) | Skip (dp\_skip) |
| ----- | ------------- | --------------- |
| 0     | 2             | 0               |
| 1     | 7             | 2               |
| 2     | 11 (2+9)      | 7               |
| 3     | 10 (7+3)      | 11              |
| 4     | 12 (11+1)     | 11              |

✅ Final Answer: **max(12, 11) = 12**

---

## 🧑‍💼 Interview Expectations

| 💬 Topic            | ✅ What to Explain                           |
| ------------------- | ------------------------------------------- |
| 🎯 Problem Type     | Classic **Dynamic Programming**             |
| 🧱 State Definition | `dp[i]` = max amount up to house `i`        |
| 🔁 Transition       | `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`   |
| 🧠 Choices          | Rob → skip previous; Skip → take max so far |
| ⏱ Time Complexity   | O(n)                                        |
| 📦 Space Complexity | O(n) — can optimize to O(1)                 |

---

## 🛠️ Alternate Approach: Optimized DP with Two Variables

We only need values from the **last two houses**, so we can reduce space to **O(1)**:

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        if len(nums) == 1:
            return nums[0]
        
        rob_prev = nums[0]  # Rob house 0
        skip_prev = 0       # Skip house 0

        for i in range(1, len(nums)):
            current_rob = skip_prev + nums[i]
            skip_prev = max(rob_prev, skip_prev)
            rob_prev = current_rob

        return max(rob_prev, skip_prev)
```

✅ Same logic, better space usage.

---

## 🔁 Recursive + Memoization (Top-Down DP)

Only if interviewer explicitly asks for a recursive solution.

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        from functools import lru_cache

        @lru_cache(None)
        def helper(i):
            if i >= len(nums):
                return 0
            return max(
                nums[i] + helper(i + 2),  # Rob current house
                helper(i + 1)             # Skip current house
            )

        return helper(0)
```

❌ Not as efficient unless memoized

✅ Good for building intuition on state transitions

---

## 📚 Follow-Up Questions

| ❓ Question                                | ✅ What to Say                                              |
| ----------------------------------------- | ---------------------------------------------------------- |
| What if houses are in a circle?           | That’s **House Robber II** → can't rob both first and last |
| What if you can rob 2 in a row but not 3? | Change state to track how many robbed in a row             |
| What if some houses alert neighbors?      | Custom transition logic or constraints needed              |

---

## 📌 Summary

* ✅ Use **DP** for maximum sum problems with constraints
* ✅ Use greedy only when transitions are memoryless
* ✅ Use **space optimization** if only last two states are needed
* ✅ Be confident explaining **choices at each step**

---
