# 🔢 Subsets (Power Set) - Leetcode #78

## 📘 Problem Description

> Given an integer array `nums` of **distinct integers**, return *all possible subsets (the power set)*.
> 
> The solution set **must not contain duplicate subsets**. Return the solution in any order.

### ✨ Example:

**Input:**  
`nums = [1,2,3]`

**Output:**  
`[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

---

## 🎯 What Interviewers Expect

### 1. **Clarity in Thought Process**
- Do you understand what a subset is?
- Can you give a dry-run of a few subsets manually?

### 2. **Approach Discussion**
- Do you start with a brute-force and optimize?
- Are you considering recursion, iteration, or bit manipulation?

### 3. **Trade-off Analysis**
- Time and space complexity of your solution?
- Would you use backtracking or bitmasking and why?

### 4. **Code Quality**
- Are your variable names meaningful?
- Do you handle edge cases like empty input?

---

## ✅ Approach 1: Backtracking (Recursive)

### 🔧 Intuition:
At each index, we have two choices:
- Include `nums[i]` in the current subset.
- Exclude `nums[i]`.

We explore both possibilities recursively.

### 🧠 Time Complexity:  
`O(2^n)` subsets for n elements, and each takes up to `O(n)` space when copied.

---

### ✅ Clean & Interview-Ready Code

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        total_elements = len(nums)
        current_subset = []
        all_subsets = []

        def generate_subsets(index: int):
            if index == total_elements:
                all_subsets.append(current_subset[:])  # Record a valid subset
                return

            # ❌ Don't include nums[index]
            generate_subsets(index + 1)

            # ✅ Include nums[index]
            current_subset.append(nums[index])
            generate_subsets(index + 1)

            # 🧹 Backtrack: remove the last added element
            current_subset.pop()

        generate_subsets(0)
        return all_subsets
````

---

## 🔁 Approach 2: Iterative (Using Expanding List)

### 💡 Logic:

Start with an empty subset. For each number, add it to all existing subsets to form new ones.

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        subsets = [[]]  # Start with empty subset

        for num in nums:
            # For every existing subset, add the new number to it
            new_subsets = []
            for existing in subsets:
                new_subsets.append(existing + [num])
            subsets.extend(new_subsets)

        return subsets
```

### 🧠 Time Complexity: `O(2^n)`

Space Complexity: `O(2^n)` (for storing all subsets)

### ✅ Why It's Interview-Friendly

* Easy to visualize
* Doesn’t use recursion (good for stack-averse systems)
* Great alternative if recursion is discouraged

---

## ⚙️ Approach 3: Bit Manipulation (Binary Masking)

### 💡 Idea:

Each subset corresponds to a binary number from `0` to `2^n - 1`. Each bit determines whether to include an element.

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        power_set = []

        for mask in range(1 << n):  # from 0 to 2^n - 1
            subset = []
            for i in range(n):
                if mask & (1 << i):  # if i-th bit is set
                    subset.append(nums[i])
            power_set.append(subset)

        return power_set
```

### 🧠 Time Complexity: `O(n * 2^n)`

* Each of `2^n` subsets takes up to `n` operations.

---

## 🗣️ What to Say in Interviews

* **"At each step, I either include or exclude the current element, forming a binary decision tree of depth `n`, hence `2^n` subsets."**
* **"I used backtracking to avoid unnecessary copying and pop to revert the state, maintaining space efficiency."**
* **"If recursion is not preferred, I can also solve this iteratively or with bitmasks."**
* **"Edge case when `nums` is empty? Only subset is `[]`, which is handled."**
* **"Since input has unique elements, I don’t need to handle duplicates."**

---

## 🎯 Interview-Driven Observations

| Question                                       | Insight                                                    |
| ---------------------------------------------- | ---------------------------------------------------------- |
| **Do you understand why `2^n` subsets exist?** | Each element has 2 options: include or exclude             |
| **What is the space usage?**                   | `O(n)` per subset, total `O(n * 2^n)`                      |
| **Can you write it iteratively?**              | Yes – show Approach 2                                      |
| **How does backtracking help?**                | Saves space by modifying and reverting a single path       |
| **Is bitmasking faster?**                      | Conceptually cleaner and iterative, useful for limited `n` |

---

## 🏁 Final Tips

* Practice dry-run for `[1,2]` and `[1,2,3]` inputs
* Visualize recursion tree and subset generation
* Always restore state after recursive calls (backtracking)
* Talk through the base case and recursive expansion
* Mention iterative and bitmasking as alternatives

---

