# 🧠 Letter Case Permutation — Interview Prep Guide

### ✅ Problem Description

**Leetcode 784 — Letter Case Permutation (Medium)**
Given a string `s`, you need to return a list of all possible strings we can form by changing the case (upper/lower) of **each letter** independently.

Digits **remain unchanged**.

---

### 🔍 Example

```python
Input: s = "a1b2"

Output: ["a1b2", "a1B2", "A1b2", "A1B2"]
```

---

### 🎯 Objective

Generate **all case permutations** of a string, where each alphabet can be either lowercase or uppercase. The string may contain **letters and digits**, and the **order of characters should remain the same**.

---

## 🧠 Interviewer Expectations

| What They Look For                    | What You Should Demonstrate                          |
| ------------------------------------- | ---------------------------------------------------- |
| Understanding of **DFS/Backtracking** | Recursive approach with decision-making at each char |
| **Iterative BFS-style** exploration   | Clean iterative build-up of results                  |
| Handling **both digits and letters**  | Correct branching based on character type            |
| Space-time trade-offs                 | Clarity about space usage for storing permutations   |

---

## 🧪 Step-by-Step Dry Run (Iterative Approach)

Let’s trace `s = "a1b"`

```
Step 0: output = [""]
Step 1: c = 'a'
        output = ['a', 'A']
Step 2: c = '1'
        output = ['a1', 'A1'] (digit → append as is)
Step 3: c = 'b'
        output = ['a1b', 'a1B', 'A1b', 'A1B']
```

---

## 🧼 Cleaned Up & Interview-Ready Code (Iterative)

```python
from typing import List

class Solution:
    def letterCasePermutation(self, input_str: str) -> List[str]:
        # Start with an empty string in the result list
        result_permutations = [""]

        # Iterate through each character in the input string
        for char in input_str:
            current_level = []

            # If the character is a letter, create both lowercase and uppercase versions
            if char.isalpha():
                for permutation in result_permutations:
                    current_level.append(permutation + char.lower())
                    current_level.append(permutation + char.upper())
            else:
                # If it's a digit, simply append it to all existing permutations
                for permutation in result_permutations:
                    current_level.append(permutation + char)
            
            # Update result with new permutations
            result_permutations = current_level

        return result_permutations
```

---

## 🌀 Alternate Approach: DFS (Backtracking)

Use recursion to explore each possibility — great for demonstrating recursion skills.

```python
class Solution:
    def letterCasePermutation(self, s: str) -> List[str]:
        result = []

        def backtrack(index: int, path: str):
            if index == len(s):
                result.append(path)
                return
            
            if s[index].isalpha():
                backtrack(index + 1, path + s[index].lower())
                backtrack(index + 1, path + s[index].upper())
            else:
                backtrack(index + 1, path + s[index])

        backtrack(0, "")
        return result
```

---

## 📘 Notes to Share in an Interview

### Observations:

* Number of permutations is `2^L`, where L is the number of letters.
* Digits don’t increase the branching.
* Order of characters remains **unchanged**.
* Handles empty string correctly (returns `[""]`).

### Trade-offs:

* **Iterative** approach: Simpler to trace and interview-friendly.
* **Recursive** approach: Natural fit for branching problems but uses more stack space.

### Time & Space Complexity:

* **Time**: `O(2^L * N)` where L = number of letters, N = length of string
* **Space**: Same as time due to storing all permutations

---

## ✨ Follow-up Questions

* Can you write a **generator** that yields permutations one by one?
* How would you handle **very long strings** efficiently?
* What if we want to **limit the number of outputs** (e.g., first 10 permutations)?

---

## 🧠 Cheat Sheet Summary

| Technique           | Notes                                   |
| ------------------- | --------------------------------------- |
| BFS / Iterative     | Build permutations level by level       |
| DFS / Backtracking  | Make recursive decisions at each letter |
| isalpha()           | Use to check if a character is a letter |
| String immutability | Use `+` to build strings in path        |

---
