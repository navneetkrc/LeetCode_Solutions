
---

# 🧩 Problem 22: Generate Parentheses

Given `n` pairs of parentheses, generate **all combinations of well-formed parentheses**.

---

## ✅ Constraints

* 1 ≤ n ≤ 8

---

## 💡 Idea

We use **backtracking** to build all valid combinations of parentheses:

* At each step:

  * We can **add a `(`** if we still have some left to place.
  * We can **add a `)`** if the number of placed `)` is less than the number of `(` used.

The base case is when no more parentheses are left to add (`left == 0 and right == 0`).

---

## ✨ Example

### Input:

```
n = 3
```

### Output:

```python
["((()))", "(()())", "(())()", "()(())", "()()()"]
```

---

## 🧠 Backtracking Strategy

* Keep track of:

  * How many **left parentheses** (`(`) are remaining.
  * How many **right parentheses** (`)`) are remaining.
  * The **current string** being built.

---

## 🧪 Python Code (Well-Commented)

```python
from typing import List

class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        # This will store the final list of valid combinations
        result = []

        # Helper function to perform backtracking
        def backtrack(left_remaining: int, right_remaining: int, current_string: List[str]):
            # Base case: when no parentheses are remaining to add
            if left_remaining == 0 and right_remaining == 0:
                result.append("".join(current_string))  # Add a copy of the built string
                return

            # Add a left parenthesis if we still have some remaining
            if left_remaining > 0:
                current_string.append("(")  # Choose
                backtrack(left_remaining - 1, right_remaining, current_string)  # Explore
                current_string.pop()  # Un-choose (backtrack)

            # Add a right parenthesis only if it won’t lead to imbalance
            if right_remaining > left_remaining:
                current_string.append(")")
                backtrack(left_remaining, right_remaining - 1, current_string)
                current_string.pop()

        # Initialize the recursion
        backtrack(n, n, [])

        return result
```

---

## 📝 Explanation with Example (`n = 2`)

* Start: `("", 2 left, 2 right)`
* Add `(` → `("(", 1 left, 2 right)`
* Add `(` → `("((", 0 left, 2 right)`
* Add `)` → `"(()", 0 left, 1 right)`
* Add `)` → `"(())"` ✅
* Backtrack and explore other paths...

---

## 🔁 Time & Space Complexity

* **Time:** O(2^2n), but only valid sequences are considered, so it's actually bounded by **Catalan number Cₙ**.
* **Space:** O(n) for the recursion stack + O(Cₙ \* n) for storing results.

---

## 📌 Key Concepts

* ✅ **Backtracking**
* ✅ **Balanced Parentheses**
* ✅ **Recursive Tree Search**
* ✅ **Pruning Invalid Paths Early**

---

