
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
# 🔗 Generate Parentheses - LeetCode Problem 22

## 📋 Problem Statement

**Difficulty:** Medium

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

### Examples

**Example 1:**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**
```
Input: n = 1
Output: ["()"]
```

### Constraints
- `1 <= n <= 8`

---

## 🎯 Solution Approach

This problem uses **backtracking** to generate all valid combinations of parentheses. The key insight is that we need to ensure:

1. We don't use more opening parentheses than `n`
2. We don't use more closing parentheses than opening ones (to maintain validity)
3. We use exactly `n` pairs in total

### 🧠 Algorithm Logic

- **Track remaining counts**: Keep track of how many opening and closing parentheses we still need to place
- **Backtrack with constraints**: At each step, decide whether to add an opening or closing parenthesis based on validity rules
- **Base case**: When no more closing parentheses are needed, we have a complete valid combination

---

## 💻 Clean Implementation

```python
from typing import List

class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        """
        Generate all combinations of well-formed parentheses for n pairs.
        
        Args:
            n: Number of parentheses pairs to generate
            
        Returns:
            List of all valid parentheses combinations
        """
        all_combinations = []
        current_combination = []
        
        def backtrack(remaining_open: int, remaining_close: int):
            """
            Recursively build valid parentheses combinations using backtracking.
            
            Args:
                remaining_open: Number of opening parentheses left to place
                remaining_close: Number of closing parentheses left to place
            """
            # Base case: All parentheses have been placed
            if remaining_close == 0:
                # Convert current combination to string and add to results
                all_combinations.append("".join(current_combination))
                return
            
            # Option 1: Add opening parenthesis (if we still have some left)
            if remaining_open > 0:
                current_combination.append("(")
                backtrack(remaining_open - 1, remaining_close)
                current_combination.pop()  # Remove for backtracking
            
            # Option 2: Add closing parenthesis (only if it won't make string invalid)
            # We can only add ')' if we have more '(' than ')' so far
            if remaining_open < remaining_close:
                current_combination.append(")")
                backtrack(remaining_open, remaining_close - 1)
                current_combination.pop()  # Remove for backtracking
        
        # Start the backtracking process with n opening and n closing parentheses
        backtrack(n, n)
        return all_combinations
```

---

## 🔍 Step-by-Step Walkthrough

Let's trace through the algorithm with `n = 2`:

### Initial State
- `remaining_open = 2`, `remaining_close = 2`
- `current_combination = []`

### Decision Tree
```
                    start(2,2)
                   /          \
               add '('      (can't add ')' yet)
              /
         (1,2) "("
        /         \
   add '('      add ')'
   /               \
(0,2) "(("      (1,1) "()"
   |               /    \
add ')'        add '('  add ')'
   |           /           \
(0,1) "(()"  (0,1) "()("  (1,0) "())"
   |            |         (invalid)
add ')'      add ')'
   |            |
(0,0) "(())"  (0,0) "()()"
```

### Valid Results
- `"(())"`
- `"()()"`

---

## ⚡ Key Insights

### Why This Works

1. **Constraint Management**: By tracking remaining counts, we ensure we never exceed `n` pairs
2. **Validity Check**: The condition `remaining_open < remaining_close` ensures we never have more closing than opening parentheses at any point
3. **Backtracking**: We explore all possibilities by adding a character, recursing, then removing it to try other options

### Time & Space Complexity

- **Time Complexity**: O(4ⁿ/√n) - This is the nth Catalan number
- **Space Complexity**: O(n) - Maximum recursion depth is 2n

---

## 🚀 Alternative Approaches

### Approach 1: Iterative with Stack
```python
def generateParenthesis_iterative(self, n: int) -> List[str]:
    """Alternative iterative approach using a stack."""
    result = []
    stack = [("", 0, 0)]  # (current_string, open_count, close_count)
    
    while stack:
        current, open_count, close_count = stack.pop()
        
        if len(current) == 2 * n:
            result.append(current)
            continue
            
        # Add opening parenthesis
        if open_count < n:
            stack.append((current + "(", open_count + 1, close_count))
            
        # Add closing parenthesis
        if close_count < open_count:
            stack.append((current + ")", open_count, close_count + 1))
    
    return result
```

### Approach 2: Mathematical (Catalan Numbers)
This problem generates the nth Catalan number of combinations, which can also be solved using dynamic programming or mathematical formulas.

---

## 🧪 Test Cases

```python
def test_generate_parentheses():
    solution = Solution()
    
    # Test case 1
    assert solution.generateParenthesis(1) == ["()"]
    
    # Test case 2
    result_n3 = solution.generateParenthesis(3)
    expected_n3 = ["((()))","(()())","(())()","()(())","()()()"]
    assert set(result_n3) == set(expected_n3)
    
    # Test case 3
    result_n2 = solution.generateParenthesis(2)
    expected_n2 = ["(())","()()"]
    assert set(result_n2) == set(expected_n2)
    
    print("All test cases passed! ✅")
```

---

## 💡 Tips for Similar Problems

1. **Backtracking Pattern**: Add element → Recurse → Remove element
2. **Constraint Tracking**: Always maintain counters for remaining resources
3. **Early Termination**: Use base cases to stop recursion when solution is complete
4. **Validity Checks**: Ensure each step maintains problem constraints

This solution demonstrates the power of backtracking for generating all valid combinations while respecting complex constraints!
