# Dynamic Programming Interview Questions – Greg Hogg Playlist

---

## Table of Contents

1. [Fibonacci Number](#fibonacci-number)
2. [Climbing Stairs](#climbing-stairs)
3. [Min Cost Climbing Stairs](#min-cost-climbing-stairs)
4. [House Robber](#house-robber)
5. [Unique Paths](#unique-paths)
6. [Coin Change](#coin-change)
7. [Maximum Subarray (Kadane's Algorithm)](#maximum-subarray-kadanes-algorithm)
8. [Jump Game](#jump-game)
9. [Longest Increasing Subsequence](#longest-increasing-subsequence)
10. [Longest Common Subsequence](#longest-common-subsequence)

---

## 1. Fibonacci Number

- **Description:** Calculate the nth Fibonacci number using dynamic programming to optimize the naive recursive approach[1].
- **LeetCode:** [LeetCode 509](https://leetcode.com/problems/fibonacci-number/)[1].
- **Companies:** Google, Amazon, Microsoft[1].

---

## 2. Climbing Stairs

- **Description:** Count the number of distinct ways to climb to the top of a staircase with n steps, taking 1 or 2 steps at a time[1].
- **LeetCode:** [LeetCode 70](https://leetcode.com/problems/climbing-stairs/)[1].
- **Companies:** Amazon, Microsoft, Google[1].

---

## 3. Min Cost Climbing Stairs

- **Description:** Find the minimum cost to reach the top of the stairs given an array of costs for each step[1].
- **LeetCode:** [LeetCode 746](https://leetcode.com/problems/min-cost-climbing-stairs/)[1].
- **Companies:** Amazon, Google[1].

---

## 4. House Robber

- **Description:** Maximize the amount of money robbed from houses without robbing two adjacent houses[1].
- **LeetCode:** [LeetCode 198](https://leetcode.com/problems/house-robber/)[1].
- **Companies:** Amazon, Facebook[1].

---

## 5. Unique Paths

- **Description:** Count the number of unique paths from the top-left to the bottom-right of a grid, moving only right or down[1].
- **LeetCode:** [LeetCode 62](https://leetcode.com/problems/unique-paths/)[1].
- **Companies:** Google, Amazon[1].

---

## 6. Coin Change

- **Description:** Find the fewest number of coins needed to make up a given amount from a set of coin denominations[1].
- **LeetCode:** [LeetCode 322](https://leetcode.com/problems/coin-change/)[1].
- **Companies:** Amazon, Google, Facebook[1].

---

## 7. Maximum Subarray (Kadane's Algorithm)

- **Description:** Find the contiguous subarray with the largest sum within an array[1].
- **LeetCode:** [LeetCode 53](https://leetcode.com/problems/maximum-subarray/)[1].
- **Companies:** Google, Amazon[1].

---

## 8. Jump Game

- **Description:** Determine if you can reach the last index of an array where each element represents the maximum jump length[1].
- **LeetCode:** [LeetCode 55](https://leetcode.com/problems/jump-game/)[1].
- **Companies:** Amazon, Microsoft[1].

---

## 9. Longest Increasing Subsequence

- **Description:** Find the length of the longest strictly increasing subsequence in an array[1].
- **LeetCode:** [LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/)[1].
- **Companies:** Google, Amazon[1].

---

## 10. Longest Common Subsequence

- **Description:** Find the length of the longest subsequence common to two strings[1].
- **LeetCode:** [LeetCode 1143](https://leetcode.com/problems/longest-common-subsequence/)[1].
- **Companies:** Amazon, Microsoft[1].

---



## Dynamic Programming Interview Tips

- **Approach:** Start with a naive recursive solution, then optimize using memoization (top-down DP), followed by bottom-up tabulation[1].
- **Optimization:** Aim to reduce time complexity from exponential to polynomial[1].
- **Space:** Optimize space by using rolling arrays or constant space when possible[1].
- **Common Patterns:** Overlapping subproblems, optimal substructure, and state definition[1].

---

# Dynamic Programming Interview Questions Solutions – Greg Hogg Playlist

---

## Table of Contents

1. [Fibonacci Number](#1-fibonacci-number)
2. [Climbing Stairs](#2-climbing-stairs)
3. [Min Cost Climbing Stairs](#3-min-cost-climbing-stairs)
4. [House Robber](#4-house-robber)
5. [Unique Paths](#5-unique-paths)
6. [Coin Change](#6-coin-change)
7. [Maximum Subarray (Kadane's Algorithm)](#7-maximum-subarray-kadanes-algorithm)
8. [Jump Game](#8-jump-game)
9. [Longest Increasing Subsequence](#9-longest-increasing-subsequence)
10. [Longest Common Subsequence](#10-longest-common-subsequence)

---

## 1. Fibonacci Number

* **Description:** Calculate the nth Fibonacci number using dynamic programming.
* **LeetCode:** [LeetCode 509](https://leetcode.com/problems/fibonacci-number/)
* **Companies:** Google, Amazon, Microsoft
* **Key Observation:** Use memoization or bottom-up DP to eliminate redundant recursive calls.
* **Interview Tip:** Highlight base cases and recursive relation during explanation.

```python
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        dp = [0, 1]
        for i in range(2, n+1):
            dp.append(dp[-1] + dp[-2])
        return dp[n]
```

---

## 2. Climbing Stairs

* **Description:** Count the number of ways to reach the top of n stairs taking 1 or 2 steps.
* **LeetCode:** [LeetCode 70](https://leetcode.com/problems/climbing-stairs/)
* **Companies:** Amazon, Microsoft, Google
* **Key Observation:** The number of ways to reach step `n` = ways to step `n-1` + ways to step `n-2`.
* **Interview Tip:** Recognize the similarity with Fibonacci sequence.

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        a, b = 1, 1
        for _ in range(n):
            a, b = b, a + b
        return a
```

---

## 3. Min Cost Climbing Stairs

* **Description:** Minimize the cost to reach the top of the stairs.
* **LeetCode:** [LeetCode 746](https://leetcode.com/problems/min-cost-climbing-stairs/)
* **Companies:** Amazon, Google
* **Key Observation:** Minimum cost to reach step `i` = cost\[i] + min(cost to reach i-1, i-2).
* **Interview Tip:** Use a bottom-up approach with constant space.

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        a, b = 0, 0
        for i in cost:
            a, b = b, i + min(a, b)
        return min(a, b)
```

---

## 4. House Robber

* **Description:** Maximize loot without robbing two adjacent houses.
* **LeetCode:** [LeetCode 198](https://leetcode.com/problems/house-robber/)
* **Companies:** Amazon, Facebook
* **Key Observation:** At each house, choose max of robbing it + `dp[i-2]` or skipping it (`dp[i-1]`).
* **Interview Tip:** Explain state transition clearly.

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        prev1 = prev2 = 0
        for num in nums:
            temp = prev1
            prev1 = max(prev2 + num, prev1)
            prev2 = temp
        return prev1
```

---

## 5. Unique Paths

* **Description:** Count unique paths in an m x n grid from top-left to bottom-right.
* **LeetCode:** [LeetCode 62](https://leetcode.com/problems/unique-paths/)
* **Companies:** Google, Amazon
* **Key Observation:** Paths to cell (i,j) = paths from (i-1,j) + (i,j-1).
* **Interview Tip:** Discuss grid traversal patterns and boundary conditions.

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1]*n for _ in range(m)]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
```

---

## 6. Coin Change

* **Description:** Find the fewest number of coins to make a given amount.
* **LeetCode:** [LeetCode 322](https://leetcode.com/problems/coin-change/)
* **Companies:** Amazon, Google, Facebook
* **Key Observation:** dp\[i] = min(dp\[i], dp\[i - coin] + 1).
* **Interview Tip:** Explain how unbounded knapsack relates to this.

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0
        for coin in coins:
            for x in range(coin, amount + 1):
                dp[x] = min(dp[x], dp[x - coin] + 1)
        return dp[amount] if dp[amount] != float('inf') else -1
```

---

## 7. Maximum Subarray (Kadane's Algorithm)

* **Description:** Find the maximum sum of a contiguous subarray.
* **LeetCode:** [LeetCode 53](https://leetcode.com/problems/maximum-subarray/)
* **Companies:** Google, Amazon
* **Key Observation:** Keep track of max current sum and global max.
* **Interview Tip:** Demonstrate greedy + dynamic hybrid reasoning.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = current = nums[0]
        for num in nums[1:]:
            current = max(num, current + num)
            max_sum = max(max_sum, current)
        return max_sum
```

---

## 8. Jump Game

* **Description:** Can you reach the last index by jumping at most nums\[i] steps?
* **LeetCode:** [LeetCode 55](https://leetcode.com/problems/jump-game/)
* **Companies:** Amazon, Microsoft
* **Key Observation:** Maintain the furthest index reachable so far.
* **Interview Tip:** Show greedy reasoning for optimality.

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        reach = 0
        for i, num in enumerate(nums):
            if i > reach:
                return False
            reach = max(reach, i + num)
        return True
```

---

## 9. Longest Increasing Subsequence

* **Description:** Length of the longest strictly increasing subsequence.
* **LeetCode:** [LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/)
* **Companies:** Google, Amazon
* **Key Observation:** Use binary search for O(n log n) solution.
* **Interview Tip:** Start with DP, then optimize with patience sorting.

```python
import bisect
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        sub = []
        for x in nums:
            i = bisect.bisect_left(sub, x)
            if i == len(sub):
                sub.append(x)
            else:
                sub[i] = x
        return len(sub)
```

---

## 10. Longest Common Subsequence

* **Description:** Length of longest subsequence common to two strings.
* **LeetCode:** [LeetCode 1143](https://leetcode.com/problems/longest-common-subsequence/)
* **Companies:** Amazon, Microsoft
* **Key Observation:** dp\[i]\[j] = dp\[i-1]\[j-1] + 1 if match, else max(dp\[i-1]\[j], dp\[i]\[j-1])
* **Interview Tip:** Clarify how subsequence differs from substring.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0]*(n+1) for _ in range(m+1)]
        for i in range(m):
            for j in range(n):
                if text1[i] == text2[j]:
                    dp[i+1][j+1] = dp[i][j] + 1
                else:
                    dp[i+1][j+1] = max(dp[i][j+1], dp[i+1][j])
        return dp[m][n]
```

---

## Dynamic Programming Interview Tips

* **Approach:** Begin with brute force or recursion. Optimize using memoization (top-down), then move to tabulation (bottom-up).
* **Patterns:** Look for overlapping subproblems and optimal substructure.
* **Space Optimization:** Use rolling arrays or variables when possible.
* **Presentation:** Explain your recursive formula and transition states clearly.
* **Edge Cases:** Think about empty arrays, 0 values, and boundary constraints.

---
