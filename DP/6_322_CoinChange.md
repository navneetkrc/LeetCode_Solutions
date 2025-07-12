# 🪙 Leetcode 322: Coin Change

## 📘 Problem Description

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the **fewest number of coins** that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an **infinite number** of each kind of coin.

---

### 🧠 Example 1:

```

Input: coins = \[1, 2, 5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

```

### 🧠 Example 2:

```

Input: coins = \[2], amount = 3
Output: -1

```

### 🧠 Example 3:

```

Input: coins = \[1], amount = 0
Output: 0

````

---

## 🛠️ Approach: Dynamic Programming (Bottom-Up)

This is a **classic unbounded knapsack** problem. You can pick a coin multiple times, so we aim to **minimize the number of coins** used to reach the target amount.

### 💡 Intuition

- `dp[x]` stores the **minimum number of coins** needed to make up amount `x`.
- Initialize `dp[0] = 0` since 0 coins are needed to make amount 0.
- For all amounts `1...amount`, check if you can use each coin:
  - If `coin <= current_amount`, try using it and update `dp[current_amount]`.

---

## ✅ Optimized Python Code with Clear Variable Names

```python
from typing import List

class Solution:
    def coinChange(self, coins: List[int], target_amount: int) -> int:
        # Initialize a DP array to store minimum coins for each amount
        # We set all values to target_amount + 1, an impossible high value
        min_coins_needed = [target_amount + 1] * (target_amount + 1)
        
        # Base case: 0 coins are needed to make amount 0
        min_coins_needed[0] = 0

        # Compute minimum coins required for all sub-amounts from 1 to target_amount
        for current_amount in range(1, target_amount + 1):
            for coin in coins:
                if coin <= current_amount:
                    remaining_amount = current_amount - coin
                    min_coins_needed[current_amount] = min(
                        min_coins_needed[current_amount],
                        1 + min_coins_needed[remaining_amount]
                    )

        # If value wasn't updated, return -1
        return min_coins_needed[target_amount] if min_coins_needed[target_amount] != target_amount + 1 else -1
````

---

## 🔍 Interview Expectations & Talking Points

### What you should highlight:

| Point                  | Explanation                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------- |
| ✅ **Problem Type**     | It's a classic **Unbounded Knapsack** problem.                                          |
| ✅ **DP Array Meaning** | Clearly explain what `dp[i]` represents — *minimum number of coins to make amount `i`*. |
| ✅ **Base Case**        | `dp[0] = 0` — it takes 0 coins to make amount 0.                                        |
| ✅ **Choice Diagram**   | At each step, try each coin and update the state if it helps minimize the result.       |
| ✅ **Time Complexity**  | O(`amount` × `n`) where `n` is the number of coin types.                                |
| ✅ **Space Complexity** | O(`amount`) for the 1D DP array.                                                        |

---

## 🔁 Alternative Approach: Top-Down DP with Memoization

Useful if the interviewer wants a recursive perspective.

```python
from functools import lru_cache

class Solution:
    def coinChange(self, coins: List[int], target_amount: int) -> int:
        @lru_cache(maxsize=None)
        def dfs(remaining):
            if remaining < 0:
                return float('inf')
            if remaining == 0:
                return 0
            return min(1 + dfs(remaining - coin) for coin in coins)

        result = dfs(target_amount)
        return result if result != float('inf') else -1
```

### ✅ Pros:

* Elegant and easy to understand
* Uses memoization to prevent recomputation

### ❌ Cons:

* Can cause recursion depth issues for large `amount` values

---

## 📊 Dry Run Example

For `coins = [1, 2, 5]`, `amount = 11`:

| Amount    | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| --------- | - | - | - | - | - | - | - | - | - | -- | -- |
| Min Coins | 1 | 1 | 2 | 2 | 1 | 2 | 2 | 3 | 3 | 2  | 3  |

Path: 5 + 5 + 1 → 3 coins total ✅

---

## 📌 Final Notes

* Always explain **base case**, **transition**, and **goal state**.
* If time allows, mention space optimization tricks (though not necessary here).
* Show confidence with recurrence relations and memoization.

---

## 🧑‍🏫 Interviewer Might Ask:

* Can you return the **actual coins used**? (Backtrack from `dp[]`)
* What if coins have **limited supply**? (Turns into bounded knapsack)
* What if coin denominations are **floating-point**? (Requires different approach)

---
