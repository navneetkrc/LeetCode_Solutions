# 🦘 Leetcode 55: Jump Game

---

## 📘 Problem Description

You are given an integer array `nums`. You are **initially at index 0**, and each element `nums[i]` represents your **maximum jump length** from that position.

Return `true` if you can **reach the last index**, otherwise return `false`.

---

### 🧠 Example 1:
```

Input: nums = \[2,3,1,1,4]
Output: true
Explanation: Jump 1 step to index 1 (value = 3), then jump 3 steps to the last index.

```

### 🧠 Example 2:
```

Input: nums = \[3,2,1,0,4]
Output: false
Explanation: You get stuck at index 3 — jump length is 0.

````

---

## 🔍 Constraints

- `1 <= nums.length <= 10⁴`
- `0 <= nums[i] <= 10⁵`

---

## 🧠 Intuition

Think of this as a **greedy reachability** problem:

- From each index, you can reach up to `index + nums[index]`.
- If at any point your reach **doesn’t go beyond the current index**, you're **stuck**.

---

## ✅ Optimal Greedy Solution (O(n) Time, O(1) Space)

```python
from typing import List

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # This tracks the furthest index we can currently reach
        furthest_reachable_index = 0

        # Traverse each index and its jump length
        for current_index, jump_length in enumerate(nums):
            # If the current index is beyond what we can reach, we're stuck
            if current_index > furthest_reachable_index:
                return False

            # Update the furthest we can go from here
            furthest_reachable_index = max(furthest_reachable_index, current_index + jump_length)

        # If loop completes, we can reach the end
        return True
````

---

## 🧪 Dry Run

### Input: `[2,3,1,1,4]`

| Index | Value | Furthest Reachable | Can Proceed? |
| ----- | ----- | ------------------ | ------------ |
| 0     | 2     | max(0, 0+2) = 2    | ✅            |
| 1     | 3     | max(2, 1+3) = 4    | ✅            |
| 2     | 1     | max(4, 2+1) = 4    | ✅            |
| 3     | 1     | max(4, 3+1) = 4    | ✅            |
| 4     | 4     | max(4, 4+4) = 8    | ✅ ✅ ✅ 🎉     |

✅ Successfully reached or passed the last index.

---

## 🧑‍💼 Interviewer Expectations

| 💬 Topic            | 💡 Explain This                                         |
| ------------------- | ------------------------------------------------------- |
| 🎯 Problem Type     | Greedy / Dynamic Programming                            |
| ⛔ Stuck Condition   | If `i > furthest_reachable_index`, return `False`       |
| 📈 Update Strategy  | Update max reach with `max(current_reach, i + nums[i])` |
| 🕓 Time Complexity  | O(n)                                                    |
| 📦 Space Complexity | O(1)                                                    |

---

## 🔁 Alternate Approach 1: Backward Greedy (Jump Destination)

Instead of going forward, go **backward** from the last index and check if we can eventually jump **back to 0**.

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        last_good_position = len(nums) - 1

        for i in range(len(nums) - 2, -1, -1):
            if i + nums[i] >= last_good_position:
                last_good_position = i

        return last_good_position == 0
```

✅ Same time and space complexity (O(n), O(1)), more intuitive in some cases.

---

## 🔁 Alternate Approach 2: DP with Memoization (Top-Down)

This is **slower** and more like brute force with caching.

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        from functools import lru_cache

        @lru_cache(None)
        def can_reach(index):
            if index >= len(nums) - 1:
                return True

            max_jump = nums[index]
            for step in range(1, max_jump + 1):
                if can_reach(index + step):
                    return True

            return False

        return can_reach(0)
```

* ❌ Time: O(n²) worst-case
* ✅ Conceptually useful for follow-up questions

---

## 📚 Follow-Up Questions

| ❓ Question                                        | ✅ What to Say                                     |
| ------------------------------------------------- | ------------------------------------------------- |
| What if you want the **minimum number of jumps**? | This becomes **Jump Game II** (Leetcode 45)       |
| What if jumps have **costs** or **weights**?      | Use Dijkstra or BFS                               |
| What if `nums` contains **negative numbers**?     | Original greedy logic breaks; custom logic needed |

---

## 📌 Summary

* ✅ Use **Greedy Forward Reachability** for O(n) solution.
* ✅ Explain "Can I reach this index?" & "How far can I go?"
* ✅ Memorize `max(i + nums[i])` pattern — shows up often.
* ✅ Mention `Jump Game II` as follow-up.

---
