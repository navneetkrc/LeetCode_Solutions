# 🔁 Leetcode 46: Permutations

## 🧩 Problem Description

Given an array `nums` of **distinct integers**, return _all the possible permutations_ of the array.  
You may return the answer in **any order**.

---

### 💡 Example

```python
Input: nums = [1, 2, 3]
Output: 
[
  [1, 2, 3],
  [1, 3, 2],
  [2, 1, 3],
  [2, 3, 1],
  [3, 1, 2],
  [3, 2, 1]
]
````

---

## ✅ Constraints

* `1 <= nums.length <= 6`
* All the integers of `nums` are **unique**

---

## 🧠 Key Observations to Share in Interviews

1. This is a classic **backtracking** problem — we explore all paths by making choices and reverting them.
2. We stop recursion once the `current_permutation` has the same length as `nums`.
3. Since input numbers are unique, we avoid repeating any number in the current path.
4. At each step, we pick **an unused number**, add it to the path, and recurse.
5. Backtrack by **removing the last element** added — this undoes the choice for the next recursive path.

---

## 🧪 Dry Run on \[1, 2, 3]

```
Path 1 → [1] → [1,2] → [1,2,3] ✅ → Backtrack to [1,2] → [1,3] → [1,3,2] ✅  
Path 2 → [2] → [2,1] → [2,1,3] ✅ → [2,3] → [2,3,1] ✅  
Path 3 → [3] → [3,1] → [3,1,2] ✅ → [3,2] → [3,2,1] ✅
```

---

## ✅ Final Interview-Ready Code (Backtracking)

```python
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        all_permutations = []

        def generate_permutations(current_path: List[int]):
            # ✅ Base Case: Complete permutation formed
            if len(current_path) == len(nums):
                all_permutations.append(current_path[:])  # Copy current path
                return

            for candidate in nums:
                if candidate in current_path:
                    continue  # ⛔ Skip if already in current path
                current_path.append(candidate)            # Choose
                generate_permutations(current_path)       # Explore
                current_path.pop()                        # Backtrack (undo)

        generate_permutations([])
        return all_permutations

```

---

## 🧠 What You Should Explain During Interviews

| Aspect                    | What to Explain                                     |
| ------------------------- | --------------------------------------------------- |
| Recursion Tree            | Show how the call stack forms different paths       |
| Backtracking              | Emphasize the **undo step** using `.pop()`          |
| Time Complexity           | `O(n!)` permutations for `n` elements               |
| Space Complexity          | `O(n)` for recursion depth and `O(n!)` for output   |
| No In-place Modifications | We use `.append()` and `.pop()` safely              |
| Avoiding Duplicates       | We skip numbers already present in the current path |

---

## ⚡ Alternate Approach: Using `deque` for better readability (Optional)

```python
from typing import List
from collections import deque

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        all_permutations = []
        current_path = deque()

        def construct_all_permutations():
            if len(current_path) == len(nums):
                all_permutations.append(list(current_path))  # Convert deque to list
                return

            for candidate in nums:
                if candidate in current_path:
                    continue
                current_path.append(candidate)
                construct_all_permutations()
                current_path.pop()

        construct_all_permutations()
        return all_permutations

```

---

## 📌 Interview Bonus Tips

* Show the difference between **Permutation** vs **Combination** problems.
* Always validate **base case and backtracking logic**.
* Speak clearly about **time and space complexity**.
* Keep your variables **semantically meaningful** — improves code readability under pressure.

---

## 🏁 Summary

* Use backtracking to **build all possible permutations**
* Avoid duplicates by **tracking used elements**
* Always backtrack correctly after exploring a path

