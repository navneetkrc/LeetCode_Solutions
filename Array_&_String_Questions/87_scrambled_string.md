# 🧩 Leetcode 87: Scramble String

---

## 📘 Problem Description

Given two strings `s1` and `s2` of the same length, determine if `s2` is a **scrambled** version of `s1`.

A string is a **scrambled** version of another if:
- It can be constructed by recursively partitioning the string into two non-empty substrings.
- Then swapping the substrings (or not), and recursively applying the same process.

---

### 🔁 Example

```text
Input:  s1 = "great", s2 = "rgeat"
Output: true
````

### 🧠 Explanation

```
"great"
  ↓ split at 2
"gr" + "eat"
     ↘ swap ↙
   "rg" + "eat" → "rgeat"
```

✅ So, "rgeat" is a valid scramble of "great".

---

## 🚧 Constraints

* `s1.length == s2.length`
* `1 <= s1.length <= 30`
* `s1` and `s2` consist of lowercase English letters.

---

## 💡 Key Insights for Interviews

| Concept                  | Explanation                                                             |
| ------------------------ | ----------------------------------------------------------------------- |
| **Recursive Splitting**  | Try all possible partition points from `1` to `n-1`.                    |
| **Two Scenarios**        | For each split, check both **swapped** and **not swapped** cases.       |
| **Pruning with Sorting** | If the characters of s1 and s2 don't match (after sorting), skip early. |
| **Memoization**          | Cache results of subproblems to avoid recomputation (top-down DP).      |
| **Base Cases**           | When strings are equal or of length 1 (after pruning).                  |

---

## 🧠 Solution Strategy: Top-Down Memoized Recursion

### 🛠️ Approach

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        memo = {}

        def dp(s1, s2):
            if (s1, s2) in memo:
                return memo[(s1, s2)]

            # Prune with quick mismatch check
            if sorted(s1) != sorted(s2):
                return False

            if s1 == s2:
                return True

            n = len(s1)
            for i in range(1, n):
                # Case 1: No swap
                if dp(s1[:i], s2[:i]) and dp(s1[i:], s2[i:]):
                    memo[(s1, s2)] = True
                    return True

                # Case 2: Swap occurred
                if dp(s1[:i], s2[-i:]) and dp(s1[i:], s2[:-i]):
                    memo[(s1, s2)] = True
                    return True

            memo[(s1, s2)] = False
            return False

        return dp(s1, s2)
```

---

## 🎞️ Step-by-Step Illustration

### 👇 Example: `s1 = "great"`, `s2 = "rgeat"`

#### Step 1: Check if sorted("great") == sorted("rgeat") ✅

#### Step 2: Try splits i = 1 to 4

---

### Split at i = 1:

* `"g"` vs `"r"` ❌
* `"g"` vs `"t"` ❌
  → `dp("great", "rgeat")` ❌ for i=1

---

### Split at i = 2:

* `"gr"` vs `"rg"` ✅

  * `dp("gr", "rg")` → split further:

    * `"g"` vs `"g"` ✅
    * `"r"` vs `"r"` ✅
  * `dp("eat", "eat")` ✅

→ ✅ `dp("great", "rgeat") = True`

---

## 🌲 Visualizing Recursive Tree

```
dp("great", "rgeat")
├── i=1
│   ├── dp("g", "r") ❌
│   └── dp("g", "t") ❌
├── i=2 ✅
│   ├── dp("gr", "rg") ✅
│   │   ├── dp("g", "g") ✅
│   │   └── dp("r", "r") ✅
│   └── dp("eat", "eat") ✅
```

---

## 🧪 Time and Space Complexity

| Aspect | Value                    |
| ------ | ------------------------ |
| Time   | O(n⁴) (with memoization) |
| Space  | O(n³) (memo + recursion) |

---

## 💬 What to Discuss in Interviews

* ✅ Start with brute-force idea and explain recursive tree.
* ✅ Introduce pruning with `sorted(s1) != sorted(s2)`.
* ✅ Explain both swap and no-swap recursion at each index.
* ✅ Mention need for memoization to reduce complexity.
* ✅ Discuss time-space tradeoffs and DP alternatives.

---

## ✅ Final Remarks

This problem is an excellent test of:

* Deep recursive thinking.
* Handling multiple subproblem states.
* Implementing top-down dynamic programming with memoization.

---

🧠 **Pro Tip**: Interviewers love hearing when you think **pruning** and **memoization** proactively. Show that you’re not brute-forcing mindlessly!

---


