# 🧮 Combination Sum - Interview Ready Guide

## 📘 Problem Description

> **Leetcode 39. Combination Sum**  
Given an array of **distinct integers** `candidates` and a target integer `target`, return a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`.  
You may reuse the same number **unlimited times**. Return the combinations in **any order**.

### ✅ Constraints:
- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- `1 <= target <= 500`
- All elements of `candidates` are **distinct**

---

## 🧠 Key Idea

- Use **backtracking** to explore each combination recursively
- At every step, **decide whether to include or exclude** a candidate
- Backtrack when the sum exceeds the target or all elements are considered

---

## 🔎 Interview Expectations

During interviews, you should:

1. **Clarify edge cases**:
   - Can elements be reused? (**Yes**)
   - Are elements unique? (**Yes**)
   - Should combinations be unique? (**Yes**)

2. **Explain the approach** clearly:
   - Recursive DFS with backtracking
   - At each index, choose to skip or pick the current candidate
   - Use a temporary list to build combinations, backtrack after recursive calls

3. **Share pruning logic**:
   - Stop recursion if the current sum exceeds the target
   - Only record combinations when current sum equals target

4. **Talk about time complexity**:
   - Worst-case time: `O(2^target)` due to two choices at each step
   - Optimize with early stopping and pruning

---

## 🧑‍💻 Clean and Commented Code (Python)

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        # Final list to store all valid combinations
        all_valid_combinations = []

        def backtrack(start_index: int, current_combination: List[int], current_sum: int):
            # Base Case 1: If current sum equals target, store the combination
            if current_sum == target:
                all_valid_combinations.append(current_combination[:])
                return

            # Base Case 2: If current sum exceeds target, no need to proceed
            if current_sum > target:
                return

            # Explore further elements from current index onward (can reuse same element)
            for i in range(start_index, len(candidates)):
                candidate_value = candidates[i]
                
                # Choose the current candidate
                current_combination.append(candidate_value)

                # Recursive call: we can reuse the same index
                backtrack(i, current_combination, current_sum + candidate_value)

                # Backtrack: remove the last added candidate
                current_combination.pop()

        # Start backtracking from index 0 with empty combination and sum = 0
        backtrack(0, [], 0)
        return all_valid_combinations
````

---

## ✨ Sample Run Example

Input:

```python
candidates = [2, 3, 6, 7], target = 7
```

Output:

```python
[[2, 2, 3], [7]]
```

Explanation:

* `2 + 2 + 3 = 7`
* `7` alone is also a valid combination

---

## 🔁 Alternative Approach (Optional: Memoized DFS)

If performance is a concern, especially with high target values, memoization can help reduce recomputation:

```python
from functools import lru_cache

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        @lru_cache(None)
        def dfs(remaining_target, index):
            if remaining_target == 0:
                return [[]]
            if remaining_target < 0 or index == len(candidates):
                return []

            current = candidates[index]

            # Combinations including current
            include_current = dfs(remaining_target - current, index)
            include_current = [[current] + comb for comb in include_current]

            # Combinations excluding current
            exclude_current = dfs(remaining_target, index + 1)

            return include_current + exclude_current

        return dfs(target, 0)
```

---

## 🧠 Interview Tips Recap

| Aspect                 | What to Say                                               |
| ---------------------- | --------------------------------------------------------- |
| 🧠 Core Strategy       | DFS + Backtracking with decision tree: Pick or Skip       |
| ⏱️ Time Complexity     | Exponential: Worst-case `O(2^T)` (T = target)             |
| 💡 Optimization        | Early pruning when `current_sum > target`                 |
| 🔁 Reuse allowed?      | Yes, same candidate can be used unlimited times           |
| ✅ Final condition      | Add combination to result only if `current_sum == target` |
| 💬 Communicate Clearly | Verbally walk through 1-2 examples during the interview   |

---

## 🏁 Summary

This is a classic recursive + backtracking question frequently asked in **FAANG** interviews. Focus on:

* Writing clean code
* Using meaningful variable names
* Demonstrating pruning and backtracking clearly
* Explaining logic with dry-run examples

You got this! 🚀

```

