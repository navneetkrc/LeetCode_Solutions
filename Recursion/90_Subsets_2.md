
---

# 🧮 Leetcode 90: Subsets II

## ✅ Problem Statement

Given an integer array `nums` that **may contain duplicates**, return all possible **unique subsets** (the power set).

The solution must **not** contain duplicate subsets. You can return the answer in **any order**.

---

## 🧾 Examples

### Example 1

**Input:**  
`nums = [1, 2, 2]`  

**Output:**  
`[[], [1], [1,2], [1,2,2], [2], [2,2]]`

### Example 2

**Input:**  
`nums = [0]`  

**Output:**  
`[[], [0]]`

---

## 🔐 Constraints

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

---

## 🧠 Approach

- We use **Backtracking** to generate all possible subsets.
- To **avoid duplicate subsets**, we:
  - **Sort the input** array to group duplicates together.
  - **Skip duplicate elements** during recursion **if they weren't included in the previous decision step**.
- Use a `prev_included` flag to keep track of whether the previous element was added to the current subset.

---

## 💡 Key Ideas

- **Sort first:** Required to bring duplicates together.
- **Skip logic:** Only include duplicate element if it was included in the last step.
- **Use a temp list:** Maintain `current_subset` to build subsets recursively.
- **Deep copy result:** Append a copy of `current_subset` to `all_subsets` at each base case.

---

## 🧾 Python Code

### ✅ **Cleaned & Explained Version of the Code**

```python
from typing import List

class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # Sort to ensure duplicates are adjacent
        all_subsets = []    # Final result list to hold all unique subsets
        current_subset = [] # Temporary list to build subsets

        def backtrack(index: int, prev_included: bool):
            # Base case: when we reach the end of the list
            if index == len(nums):
                all_subsets.append(current_subset[:])  # Add a copy of the current subset
                return
            
            # Choice 1: Exclude the current number
            backtrack(index + 1, False)

            # Choice 2: Include the current number (only if it's not a duplicate or was included previously)
            if index > 0 and nums[index] == nums[index - 1] and not prev_included:
                return  # Skip duplicates unless the previous duplicate was included

            # Include nums[index] in the current subset
            current_subset.append(nums[index])
            backtrack(index + 1, True)  # Mark that current element is included
            current_subset.pop()        # Backtrack step: remove the last element

        # Start the recursion from index 0 with "previous included" set to False
        backtrack(0, False)
        return all_subsets
```


---

## 🧪 Complexity

* **Time Complexity:** `O(2^n)` in the worst case (with pruning reducing actual calls)
* **Space Complexity:** `O(n)` recursion depth + space for results

---

## 🧭 Summary

This solution uses **backtracking with smart pruning** to avoid duplicates, taking advantage of sorting and tracking inclusion decisions to efficiently generate all unique subsets of an array with potential duplicates.

---

