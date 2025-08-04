# 🎯 Problem: Combination Sum II

> **Leetcode 40. Medium**  
Given a collection of candidate numbers (`candidates`) and a target number (`target`), return all **unique combinations** in `candidates` where the chosen numbers sum to `target`.  
Each number may **only be used once**, and the **result must not contain duplicates**.

---

## ✅ Constraints

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

---

## 🧠 Key Concepts

| Concept              | Explanation                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **Backtracking**     | Try all possible combinations recursively                                   |
| **Avoid duplicates** | Use sorted input + skip duplicates in loop (`if i > start_index and ...`)   |
| **Use element once** | Move to next index (`i + 1`) after picking current element                  |

---

## 🔎 Sample Input / Output

### Example 1:
```python
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: [[1,1,6], [1,2,5], [1,7], [2,6]]
````

### Example 2:

```python
Input: candidates = [2,5,2,1,2], target = 5
Output: [[1,2,2], [5]]
```

---

## 🧑‍💻 Interview-Ready Code with Meaningful Names

```python
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        all_valid_combinations = []
        current_combination = []

        # Sort candidates to handle duplicates easily
        candidates.sort()

        def backtrack(start_index: int, remaining_target: int):
            # 🎯 Base Case: Found a valid combination
            if remaining_target == 0:
                all_valid_combinations.append(current_combination[:])
                return

            for i in range(start_index, len(candidates)):
                current_value = candidates[i]

                # ❌ Skip duplicates at the same recursive level
                if i > start_index and candidates[i] == candidates[i - 1]:
                    continue

                # 🔁 Prune the recursion if current value exceeds remaining target
                if current_value > remaining_target:
                    break

                # ✅ Choose the current candidate
                current_combination.append(current_value)

                # Recur with next index (each number can be used only once)
                backtrack(i + 1, remaining_target - current_value)

                # 🔙 Backtrack: remove last added element
                current_combination.pop()

        # 🚀 Start recursive backtracking
        backtrack(0, target)
        return all_valid_combinations
```

---

## 🔍 Why `i > start_index and candidates[i] == candidates[i - 1]`?

This ensures:

* You **skip duplicates** only **at the same recursive depth**
* Example: In `[1, 1, 2]`, only **one** `[1, 2]` combination is produced, not two

Visual explanation:

![Recursion Tree Skipping Duplicates](./skip_duplicates_visual.png)

---

## 🎤 What To Say in the Interview

| Point to Emphasize                       | What You Should Say                                                          |
| ---------------------------------------- | ---------------------------------------------------------------------------- |
| Use of sorting                           | “I sorted the array to group duplicates and skip them efficiently.”          |
| Avoiding duplicates                      | “I skip repeating elements at the same level using `i > start_index` check.” |
| Using each number once                   | “After picking a number, I move to `i + 1` to ensure it's used only once.”   |
| Base cases                               | “I stop exploring if the target becomes 0 (valid) or < 0 (invalid).”         |
| Backtracking logic                       | “I explore one path, then undo the choice to try alternate combinations.”    |
| Early pruning with `if current > target` | “I break early since the array is sorted and further values are too large.”  |

---

## 🧠 Alternative Approach: Set for Deduplication (Not Preferred in Interviews)

You can use a `set` to deduplicate combinations, but:

* It's **less efficient**
* You should **avoid generating duplicates in the first place**

---

## 📈 Time & Space Complexity

| Type  | Complexity             |
| ----- | ---------------------- |
| Time  | `O(2^n)` worst case    |
| Space | `O(k)` per combination |

> `n` = length of `candidates`, `k` = average combination length
> Pruning and skipping duplicates reduce actual runtime significantly.

---


Here's a **complete breakdown** of the recursion trace, visual explanation, and in-depth focus on the **deduplication logic** using the example:

---

### 🔢 Example Input:

```python
candidates = [2, 5, 2, 1, 2]
target = 5
```

### ✅ Step 1: Sort the candidates

```python
candidates.sort() → [1, 2, 2, 2, 5]
```

---

## 🔄 Backtracking Recursive Calls Trace

We’ll now simulate the recursion step by step with:

* `start_index`: where we begin the iteration
* `current_combination`: our temporary combination list
* `remaining_target`: how much more we need to reach the target sum

---

### 📍 Root Call

```python
backtrack(start_index=0, remaining_target=5, current_combination=[])
```

---

### Step-by-Step Recursion Tree (Indented for depth)

```
[] ─── pick 1
└── [1] ─── pick 2
    └── [1, 2] ─── pick 2
        └── [1, 2, 2] → remaining_target = 0 ✅ valid, add to result

        🔙 Backtrack → [1, 2]
        🔁 i = 3 (candidates[3] = 2) → duplicate of candidates[2], skip it ❌

        🔁 i = 4 → pick 5 → [1, 2, 5] → remaining_target < 0 ❌ stop

    🔙 Backtrack → [1]
    🔁 i = 2 → candidates[2] == candidates[1] → **skip duplicate** ❌
    🔁 i = 3 → same duplicate → **skip again** ❌
    🔁 i = 4 → pick 5
        └── [1, 5] → remaining_target < 0 ❌ stop

🔙 Backtrack → []

🔁 i = 1 → pick 2
    └── [2] ─── pick 2
        └── [2, 2] ─── pick 2 → sum > target ❌
        🔁 i = 3 → duplicate → skip ❌
        🔁 i = 4 → pick 5 → [2, 2, 5] → sum > target ❌

    🔁 i = 2 → duplicate of candidates[1] → **skip** ❌
    🔁 i = 3 → again duplicate → **skip** ❌
    🔁 i = 4 → pick 5
        └── [2, 5] → remaining_target < 0 ❌

🔁 i = 4 → pick 5
    └── [5] → remaining_target = 0 ✅ add to result
```

---

## ✅ Final Result:

```python
[
  [1, 2, 2],
  [5]
]
```

---

## 🎯 Explanation of Deduplication

We avoid duplicate combinations **by skipping repeated elements on the same recursive level**.

### 👇 The Key Line

```python
if i > start_index and candidates[i] == candidates[i - 1]:
    continue  # skip duplicates
```

This ensures:

* In a loop at a given recursive depth, we only use the **first occurrence** of a number
* E.g., in `[2, 2, 2]`, if we already tried `2` at `i = 1`, we skip it for `i = 2` and `i = 3`

---

## 🔍 Visual Diagram of Recursion with Skipping

We visualize the recursion tree **before skipping**:

```
                                 []
                      /          |          \
                   [1]         [2]         [5]
                  /   \        |
              [1,2]  [1,5]    [2,2]
              /        \        |
          [1,2,2]    x(out)   x(out)
```

After deduplication (skipping same values at same level):

```
                                 []
                    ↙          ↓           ↘
                [1]        [2]         [5]
                 ↓           ↓           ✅
             [1,2]       [2,2]         
               ↓                         
           [1,2,2]   ✅                
```

---

## 💡 Interviewer Expectations

Here’s how to structure your explanation:

| Topic               | What to Say                                                                            |
| ------------------- | -------------------------------------------------------------------------------------- |
| Why sort first?     | “To bring duplicates together so I can skip them efficiently.”                         |
| Skipping duplicates | “If I see the same number on the same level, I skip it using `i > start_index` check.” |
| Target check        | “If remaining target becomes 0, I store a copy of the combination.”                    |
| Pruning             | “If the current candidate exceeds remaining target, I break the loop early.”           |
| One-time use        | “I move to `i + 1` in recursion to avoid reusing the same number.”                     |

---
## ✅ Summary

This is a **classic backtracking + deduplication** problem. Focus on:

* Recursion tree
* Skipping duplicates
* One-time use of candidates
* Communicating choices and pruning clearly

---
