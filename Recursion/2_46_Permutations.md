# 🔁 Permutations – Leetcode #46

## 📘 Problem Description

> Given an array `nums` of distinct integers, return *all the possible permutations* of the array. You can return the answer in **any order**.

---

### ✨ Example:

**Input:**  
`nums = [1, 2, 3]`

**Output:**  
```

\[
\[1, 2, 3],
\[1, 3, 2],
\[2, 1, 3],
\[2, 3, 1],
\[3, 1, 2],
\[3, 2, 1]
]

````

---

## 🎯 What Interviewers Expect

1. ✅ **Understanding of Permutations**:  
   - Can you explain what a permutation is?
   - Do you know how many permutations exist for `n` elements?

2. ✅ **Backtracking Mastery**:
   - Do you understand how recursion and decision trees work?
   - Do you manage state correctly (especially undoing choices with `.pop()`)?

3. ✅ **Space-Time Complexity**:
   - Are you able to justify the `O(n!)` time?
   - Are you managing auxiliary space well?

4. ✅ **Edge Cases**:
   - What if input is empty or has one element?

---

## ✅ Approach 1: Backtracking

### 💡 Intuition:
Build up permutations by choosing one element at a time, skipping the ones we've already used.

---

### ✅ Interview-Ready Code with Explanation

```python
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        total_elements = len(nums)
        current_permutation = []
        all_permutations = []

        def generate_permutations():
            # Base case: when the current permutation is complete
            if len(current_permutation) == total_elements:
                all_permutations.append(current_permutation[:])
                return

            for num in nums:
                if num not in current_permutation:
                    # Choose the number
                    current_permutation.append(num)

                    # Explore further
                    generate_permutations()

                    # Un-choose the number (backtrack)
                    current_permutation.pop()

        generate_permutations()
        return all_permutations
````

### 🔁 Recursion Tree:

```
Start: []

├── Choose 1 → [1]
│   ├── Choose 2 → [1, 2]
│   │   └── Choose 3 → [1, 2, 3] ✅
│   │       Backtrack → [1, 2]
│   └── Choose 3 → [1, 3]
│       └── Choose 2 → [1, 3, 2] ✅
│           Backtrack → [1, 3]
│       Backtrack → [1]
│   Backtrack → [1]

├── Choose 2 → [2]
│   ├── Choose 1 → [2, 1]
│   │   └── Choose 3 → [2, 1, 3] ✅
│   │       Backtrack → [2, 1]
│   └── Choose 3 → [2, 3]
│       └── Choose 1 → [2, 3, 1] ✅
│           Backtrack → [2, 3]
│       Backtrack → [2]
│   Backtrack → [2]

└── Choose 3 → [3]
    ├── Choose 1 → [3, 1]
    │   └── Choose 2 → [3, 1, 2] ✅
    │       Backtrack → [3, 1]
    └── Choose 2 → [3, 2]
        └── Choose 1 → [3, 2, 1] ✅
            Backtrack → [3, 2]
        Backtrack → [3]
    Backtrack → []

```

---

### ✅ All Collected Permutations:

After all recursive calls and backtracks:

```python
[
  [1, 2, 3],
  [1, 3, 2],
  [2, 1, 3],
  [2, 3, 1],
  [3, 1, 2],
  [3, 2, 1]
]
```

---

### 🧠 Key Observations to Share in Interviews:

* "At each level of recursion, I'm choosing one unused number and exploring further."
* "Backtracking ensures that the current state is clean before exploring the next choice."
* "The recursion depth reaches `n = 3` when we have a complete permutation."
* "We avoid using the same number twice in the same permutation by checking if it's already in `current_permutation`."

---


---

### ⏱️ Time and Space Complexity

| Metric           | Value                           |
| ---------------- | ------------------------------- |
| Time Complexity  | `O(n!)` permutations generated  |
| Space Complexity | `O(n)` recursive depth + output |

---

## 🔁 Approach 2: Backtracking with Visited Flags

Useful for performance when `nums` is large. Avoids using `if num not in permutation`.

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        total_elements = len(nums)
        used = [False] * total_elements
        current_permutation = []
        all_permutations = []

        def backtrack():
            if len(current_permutation) == total_elements:
                all_permutations.append(current_permutation[:])
                return

            for index in range(total_elements):
                if not used[index]:
                    used[index] = True
                    current_permutation.append(nums[index])

                    backtrack()

                    used[index] = False
                    current_permutation.pop()

        backtrack()
        return all_permutations
```

---

## 🔧 Optional Approach 3: Built-in Library (Not for Interviews)

```python
import itertools

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return list(itertools.permutations(nums))
```

### ❌ But Avoid This in Interviews:

Interviewers want **your logic**, not Python magic.

---

## 🧠 What You Should Say in the Interview

* **"Since each number must appear exactly once, I'm using backtracking to explore all choices."**
* **"My base case is when the temporary list has `n` elements, i.e., a full permutation."**
* **"I remove the last element after exploring a path — this is the essence of backtracking."**
* **"I use a `visited` list in some versions to avoid repeated membership checks."**
* **"There are `n!` permutations, so time complexity is factorial."**

---

## 🗣️ Interview Conversation Starters

| Topic                        | What to Say                                                                         |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| 🧠 **Problem Understanding** | "We're generating all permutations — order matters and all elements are used once." |
| ⚙️ **Approach**              | "I'm using recursive backtracking to explore all paths."                            |
| 🔁 **State Restoration**     | "After adding an element, I backtrack by popping it out — ensuring clean state."    |
| ⏱️ **Complexity**            | "O(n!) time due to n choices, then n-1, etc. Space is O(n) for recursion + output." |
| 🧪 **Edge Cases**            | "Empty list gives `[[]]`. Single element `[1]` gives `[[1]]`."                      |

---

## 📌 Final Notes

* ✅ Stick to recursion and backtracking unless asked otherwise
* ✅ Always undo changes to temporary lists
* ✅ Avoid using `in` checks for large arrays (use flags instead)
* ✅ Practice with small inputs and simulate the recursion tree

---

## 🧪 Dry Run for `nums = [1, 2, 3]`

We use:

* `current_permutation`: a temporary list that builds the permutation
* `all_permutations`: stores all valid permutations

---


