
---

# 🧮 LeetCode 39: Combination Sum

## ✅ Problem Statement

Given an array of **distinct integers** `candidates` and a **target integer** `target`, return a list of **all unique combinations** of `candidates` where the chosen numbers sum to `target`.

* You may use the same number **multiple times**.
* The order of combinations does not matter.
* Two combinations are unique if their frequency of at least one number is different.

---

## 🧪 Example Test Cases

### 🔹 Example 1

**Input:**

```python
candidates = [2,3,6,7], target = 7
```

**Output:**

```python
[[2,2,3],[7]]
```

**Explanation:**

* `2 + 2 + 3 = 7`
* `7 = 7`

---

### 🔹 Example 2

**Input:**

```python
candidates = [2,3,5], target = 8
```

**Output:**

```python
[[2,2,2,2],[2,3,3],[3,5]]
```

---

### 🔹 Example 3

**Input:**

```python
candidates = [2], target = 1
```

**Output:**

```python
[]
```

---

## 🔒 Constraints

* `1 <= candidates.length <= 30`
* `2 <= candidates[i] <= 40`
* All elements of `candidates` are **distinct**
* `1 <= target <= 40`

---

## ⚙️ Approach: Backtracking (With Reuse)

### 💡 Intuition

We can think of this as a decision tree:

* At each step, we decide whether to **include** a candidate again (reuse) or **skip** it and move forward.
* We recurse down all valid paths until the **sum equals the target** or **exceeds it**.

---

### ✅ Clean Python Code with Comments

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []  # Store all valid combinations

        def backtrack(index: int, current_sum: int, current_combination: List[int]):
            # 🔸 Base Case 1: If sum exceeds target, stop exploring
            if current_sum > target:
                return

            # 🔸 Base Case 2: If sum matches the target, add this combination to the result
            if current_sum == target:
                result.append(current_combination[:])
                return

            # 🔸 Base Case 3: If no more candidates to check
            if index >= len(candidates):
                return
            
            # 🔹 Choice 1: Skip the current candidate and move to next
            backtrack(index + 1, current_sum, current_combination)

            # 🔹 Choice 2: Pick current candidate and stay (can be used again)
            current_combination.append(candidates[index])
            backtrack(index, current_sum + candidates[index], current_combination)
            current_combination.pop()  # Undo the pick (backtrack)

        # 🔁 Start the backtracking from index 0 and sum 0
        backtrack(0, 0, [])
        return result
```

---

## 🌲 Visualization Idea

Imagine a tree where:

* Each node represents a recursive call.
* Left branch = "skip current candidate".
* Right branch = "pick current candidate".
* A path is accepted only if the sum equals the target.

Would you like a visual tree diagram showing how combinations like `[2,2,3]` are formed?

---
Here's an extended **visual + analytical guide** to the **Combination Sum (LeetCode 39)** problem, complete with:

1. 🌲 **Recursive Tree Visualization**
2. ⏱ **Time & Space Complexity Analysis**
3. ✅ Python code recap

---

# 🔍 LeetCode 39: Combination Sum

## 🌲 Recursive Tree Visualization

### Example: `candidates = [2, 3, 6, 7]`, `target = 7`
# 🌳 Decision Tree for combinationSum([2, 3, 6, 7], target = 7)

Start: target = 7, current = []

- Choose 2 → target = 5, current = [2]
  - Choose 2 → target = 3, current = [2, 2]
    - Choose 2 → target = 1, current = [2, 2, 2]
      - Choose 2 → target = -1 ❌
      - Choose 3 → target = -2 ❌
      - Choose 6 → target = -5 ❌
      - Choose 7 → target = -6 ❌
    - Choose 3 → target = 0 ✅ [2, 2, 3]
    - Choose 6 → target = -3 ❌
    - Choose 7 → target = -4 ❌
  - Choose 3 → target = 2, current = [2, 3]
    - Choose 3 → target = -1 ❌
    - Choose 6 → target = -4 ❌
    - Choose 7 → target = -5 ❌
  - Choose 6 → target = -1 ❌
  - Choose 7 → target = -2 ❌
- Choose 3 → target = 4, current = [3]
  - Choose 3 → target = 1, current = [3, 3]
    - Choose 3 → target = -2 ❌
    - Choose 6 → target = -5 ❌
    - Choose 7 → target = -6 ❌
  - Choose 6 → target = -2 ❌
  - Choose 7 → target = -3 ❌
- Choose 6 → target = 1, current = [6]
  - Choose 6 → target = -5 ❌
  - Choose 7 → target = -6 ❌
- Choose 7 → target = 0 ✅ [7]

# ✅ Valid Combinations:
- [2, 2, 3]
- [7]



---
---

## ⏱ Time and Space Complexity

Let:

* `n = len(candidates)`
* `T = target`
* Assume values in `candidates` are small and repetitions allowed.

### **Time Complexity: `O(2^T)` (Exponential)**

* Every number can either be picked multiple times or skipped.
* In the worst case, we try every combination of candidate picks whose total does not exceed the target.
* For example, if candidates = \[1], target = 20 → you try 20 levels deep with branching = 2.

⏱ **So total nodes in recursion tree ≈ O(2^T)** (this is a loose upper bound).

> Pruning (like `if current_sum > target`) helps, but exponential growth remains.

---

### **Space Complexity: `O(T)` (Auxiliary Stack + Result Storage)**

* Stack depth = maximum path length = `O(T / min(candidates))`
* Result combinations use extra space. In worst case (if many small numbers), could return a large number of combinations (up to 150 as per constraint).

🧠 **So:**

* **Call stack space**: `O(T)` in worst case
* **Output space**: up to `O(# combinations × average combination length)`

---

## 📊 Summary

| Metric              | Value                                   |
| ------------------- | --------------------------------------- |
| Time Complexity     | `O(2^T)`                                |
| Space Complexity    | `O(T + output size)`                    |
| Pruning             | Yes                                     |
| Allows Repetition   | Yes (use same candidate multiple times) |
| Recursion Tree Type | Pick / Skip (2 branches per call)       |

---

Python doesn’t optimize for **tail recursion** (it doesn’t eliminate tail calls), but we can still **refactor** the logic in a way that resembles tail-recursive style or converts it to an **explicit stack-based iterative approach** to mimic tail recursion optimization manually.

Below are **two options**:

---

## ✅ Option 1: Cleaned Tail-Recursive Style (Conceptual)

This version keeps the recursive structure but writes it in a tail-recursive-friendly style — though Python won’t optimize tail calls, it still improves clarity and follows the tail recursion principle by ensuring the recursive call is the last action.

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []

        def backtrack(index: int, current_sum: int, current_comb: List[int]):
            if current_sum == target:
                result.append(current_comb[:])
                return
            if current_sum > target or index == len(candidates):
                return

            # Tail-recursive in structure (call is last thing done)
            
            # Include current index
            backtrack(index, current_sum + candidates[index], current_comb + [candidates[index]])
            # Exclude current index
            backtrack(index + 1, current_sum, current_comb)

        backtrack(0, 0, [])
        return result
```

**Changes:**

* `current_comb + [candidates[index]]` makes a new list for immutability (preserves tail-rec style).
* Each recursive call is clearly the final step in the logic branch.

**Trade-offs:**

* More memory used due to list copying in `current_comb + [...]`
* More readable and functional-style recursion

---

## 🔁 Option 2: Iterative DFS using Stack (Tail-recursion simulation)

If you’re looking for actual **tail-recursion optimization**, simulate it with an **explicit stack**:

```python
from typing import List

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        stack = [(0, 0, [])]  # Each element: (index, current_sum, current_comb)

        while stack:
            index, current_sum, current_comb = stack.pop()

            if current_sum == target:
                result.append(current_comb)
                continue
            if current_sum > target or index >= len(candidates):
                continue

            # Push choices in reverse order (so pick-first is processed before skip)
            # Choice to skip the current index
            stack.append((index + 1, current_sum, current_comb))

            # Choice to pick current index (allow reuse)
            stack.append((index, current_sum + candidates[index], current_comb + [candidates[index]]))

        return result
```

**Advantages:**

* Avoids recursion depth issues
* Closely mimics tail-recursive behavior
* Scales better in Python than deep recursion

---

## 📊 Time and Space Complexity

* **Time Complexity**:
  Worst case: exponential — O(2ⁿ) due to branching at each index for pick/skip
  (Exact depends on values in `candidates` and `target`)

* **Space Complexity**:

  * Recursive version: O(target) stack space (max depth)
  * Iterative version: O(#combinations + max path length) for storing all partial results

---

Would you like me to visualize the stack version as well or compare performance with `cProfile`?


            

        
