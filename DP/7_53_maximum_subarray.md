# 💥 Leetcode 53: Maximum Subarray

---

## 📘 Problem Description

Given an integer array `nums`, find the **contiguous subarray (containing at least one number)** which has the **largest sum**, and return its sum.

---

### 🧠 Example 1:
```

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4, -1, 2, 1] has the largest sum = 6

```

### 🧠 Example 2:
```

Input: nums = [1]
Output: 1

```

### 🧠 Example 3:
```

Input: nums = [5,4,-1,7,8]
Output: 23

````

---

## 🧠 Intuition

This is the classic **Kadane’s Algorithm**.

### 💡 Idea:

- At each index, decide:  
  → Do I **extend** the previous subarray, or  
  → Do I **start a new subarray** from this number?

---

## ✅ Optimized Solution (Kadane’s Algorithm)

```python
from typing import List

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Initialize current_subarray_sum and global_max_sum with the first element
        current_subarray_sum = nums[0]
        global_max_sum = nums[0]

        # Traverse from the second element onwards
        for i in range(1, len(nums)):
            current_value = nums[i]

            # If extending the subarray is better, do it; otherwise, start fresh
            current_subarray_sum = max(current_value, current_subarray_sum + current_value)

            # Update the global max if the current subarray sum is greater
            global_max_sum = max(global_max_sum, current_subarray_sum)

        return global_max_sum
````

---

## 🔍 Dry Run Example

### Input: `[-2,1,-3,4,-1,2,1,-5,4]`

| Index | Value | `current_subarray_sum` | `global_max_sum` |
| ----- | ----- | ---------------------- | ---------------- |
| 0     | -2    | -2                     | -2               |
| 1     | 1     | max(1, -2+1) = 1       | 1                |
| 2     | -3    | max(-3, 1-3) = -2      | 1                |
| 3     | 4     | max(4, -2+4) = 4       | 4                |
| 4     | -1    | max(-1, 4-1) = 3       | 4                |
| 5     | 2     | max(2, 3+2) = 5        | 5                |
| 6     | 1     | max(1, 5+1) = 6        | 6                |
| 7     | -5    | max(-5, 6-5) = 1       | 6                |
| 8     | 4     | max(4, 1+4) = 5        | 6                |

✅ Final Answer: 6

---

## 🧑‍💼 Interviewer Expectations

| 🎯 Topic             | What You Should Explain                             |
| -------------------- | --------------------------------------------------- |
| ✅ Problem Type       | This is a **Dynamic Programming** problem           |
| ✅ Subproblem         | `dp[i]` = Max sum of subarray **ending at index i** |
| ✅ Transition         | `dp[i] = max(nums[i], dp[i-1] + nums[i])`           |
| ✅ Initialization     | `dp[0] = nums[0]`                                   |
| ✅ Space Optimization | We only need the previous state → O(1) space        |
| ✅ Time Complexity    | O(n) — single pass through array                    |

---

## 🔁 Alternate Approach 1: Using a DP Array

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [0] * n
        dp[0] = nums[0]
        max_sum = nums[0]

        for i in range(1, n):
            dp[i] = max(nums[i], dp[i-1] + nums[i])
            max_sum = max(max_sum, dp[i])

        return max_sum
```

* ✅ More verbose, helps for debugging
* ❌ Space: O(n)

---

## 🔁 Alternate Approach 2: Divide & Conquer (for academic interest)

This runs in O(n log n), rarely used in interviews due to complexity.

---

## 📌 Key Observations to Share

* 📏 This is about **contiguous subarrays**
* 💣 Negative numbers can be skipped — don’t always help
* 🎯 Decision at every index: extend or start new
* 🧠 Kadane's Algorithm is **greedy + DP**
* 🧮 Track **local sum** and **global max**

---

## 📚 Follow-up Questions

| ❓ Question                         | ✅ What to Say                                     |
| ---------------------------------- | ------------------------------------------------- |
| Can we return the subarray itself? | Yes, track start & end indices while updating max |
| What if array is empty?            | Add a check — depends on constraints              |
| Can we do it in-place?             | Yes, space is already O(1)                        |

---

## 🧠 Summary

* ✅ O(n) time, O(1) space
* ✅ Classic DP + greedy hybrid
* ✅ Easy to dry-run and visualize
* ✅ Interviewers love this — *must-know problem*

---
