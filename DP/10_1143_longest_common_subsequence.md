# 🧬 Leetcode 1143: Longest Common Subsequence (LCS)

---

## 📘 Problem Description

Given two strings `text1` and `text2`, return the **length** of their **Longest Common Subsequence**.

A **subsequence** of a string is a new string generated from the original string by deleting some (or no) characters **without changing the order** of the remaining characters.

A **common subsequence** is a subsequence that appears in both strings.

---

### 🧠 Example 1:

```

Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The LCS is "ace"

```

### 🧠 Example 2:

```

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The LCS is "abc"

````

---

## 💡 Intuition

To find the **Longest Common Subsequence**, we need to make a decision at every character in both strings:

- If the characters match → include it and move diagonally  
- If not → skip a character from either `text1` or `text2` and take the max

---

## ✅ Approach 1: DP Tabulation (Bottom-Up)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m = len(text1)
        n = len(text2)

        # dp[i][j] = LCS length of text1[0:i] and text2[0:j]
        dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]

        for row in range(1, m + 1):
            for col in range(1, n + 1):
                if text1[row - 1] == text2[col - 1]:
                    # Characters match, include this in LCS
                    dp[row][col] = dp[row - 1][col - 1] + 1
                else:
                    # Take max from either skipping one character from text1 or text2
                    dp[row][col] = max(dp[row - 1][col], dp[row][col - 1])

        return dp[m][n]
````

---

### 🔍 Dry Run: `text1 = "abcde"`, `text2 = "ace"`

| i/j | "" | a | c | e |
| --- | -- | - | - | - |
| ""  | 0  | 0 | 0 | 0 |
| a   | 0  | 1 | 1 | 1 |
| b   | 0  | 1 | 1 | 1 |
| c   | 0  | 1 | 2 | 2 |
| d   | 0  | 1 | 2 | 2 |
| e   | 0  | 1 | 2 | 3 |

✅ Answer: `dp[5][3] = 3`

---

### ⏱ Time and Space Complexity

* **Time:** O(m × n)
* **Space:** O(m × n)

---

## ✅ Approach 2: Space Optimized DP (O(n) Space)

Since each row only depends on the previous row, we can reduce the space to **O(n)**.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        previous_row = [0] * (n + 1)

        for i in range(1, m + 1):
            current_row = [0] * (n + 1)
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    current_row[j] = previous_row[j - 1] + 1
                else:
                    current_row[j] = max(previous_row[j], current_row[j - 1])
            previous_row = current_row

        return previous_row[n]
```

---

## 🧑‍💼 What to Say in Interviews

| Topic                | What to Discuss                                                             |
| -------------------- | --------------------------------------------------------------------------- |
| ✅ Problem Type       | Dynamic Programming – 2D substructure                                       |
| ✅ Subproblem         | `dp[i][j]` = LCS of `text1[:i]` and `text2[:j]`                             |
| ✅ Transition         | `+1` if match, else `max(left, top)`                                        |
| ✅ Base Case          | If any string is empty → LCS = 0                                            |
| ✅ Space Optimization | From O(m×n) to O(n) using two rows                                          |
| ✅ Follow-up          | "Can you reconstruct the LCS?" (yes, using parent pointers or backtracking) |

---

## 📚 Bonus: Reconstructing the LCS

If required, we can also backtrack from `dp[m][n]` to rebuild the **actual sequence**.

This is helpful in applications like **DNA sequence alignment**, **file comparison**, etc.

---

## 📌 Summary

* Use a 2D DP table for clarity.
* Transition rule: `dp[i][j] = dp[i-1][j-1] + 1` if characters match, else `max(dp[i-1][j], dp[i][j-1])`
* Space can be optimized to O(n)
* Interviewers may ask for both length and actual subsequence

---
