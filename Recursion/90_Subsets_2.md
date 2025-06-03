
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

Here’s a **visual representation using a recursion tree** for the input `nums = [1, 2, 2]` with the backtracking logic from the `subsetsWithDup` function.

---

## 🌳 **Recursion Tree for `nums = [1, 2, 2]`**

We sort the array → `[1, 2, 2]`

Each level in the tree represents a decision:

* **Include** the current number
* **Exclude** the current number

---

## 🪜 Step-by-Step Tree Traversal

1. Start with empty subset `[]` at index `0`

   * Include `1` → `[1]`✅

     * Include `2` → `[1, 2]`

       * Include second `2` → `[1, 2, 2]` ✅
       * Exclude second `2` → `[1, 2]` ✅
     * Exclude first `2` → skip second `2` (since prev\_included = False) 🔥 Pruned
   * Exclude `1` → `[]`✅

     * Include `2` → `[2]`

       * Include second `2` → `[2, 2]` ✅
       * Exclude second `2` → `[2]` ✅
     * Exclude first `2` → skip second `2` 🔥 Pruned

---

## ✅ Final Unique Subsets Collected:

```
[[], [1], [1,2], [1,2,2], [2], [2,2]]
```

---

## 🎯 Insights:

* Pruning happens **only** when we have a **duplicate element**, and the **previous same element was skipped**.
* This ensures that each duplicate contributes **only once** for each inclusion path, preventing results like `[2, 2]` and `[2, 2]` again.

Would you like this visual as a **diagram image** (tree-style drawing)?

---

## 🧪 Complexity

* **Time Complexity:** `O(2^n)` in the worst case (with pruning reducing actual calls)
* **Space Complexity:** `O(n)` recursion depth + space for results

---

## 🧭 Summary

This solution uses **backtracking with smart pruning** to avoid duplicates, taking advantage of sorting and tracking inclusion decisions to efficiently generate all unique subsets of an array with potential duplicates.

---
### Code with comments to show the flow
```python
from typing import List

class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # Sort to bring duplicates together
        all_subsets = []    # Final result list to hold all unique subsets
        current_subset = [] # Temporary list to build subsets

        def backtrack(index: int, prev_included: bool, depth: int):
            indent = "  " * depth  # Indentation for visualization
            
            if index == len(nums):
                print(f"{indent}✅ Adding subset: {current_subset}")
                all_subsets.append(current_subset[:])
                return

            # Exclude current number
            print(f"{indent}Exclude nums[{index}] = {nums[index]}")
            backtrack(index + 1, False, depth + 1)

            # Prune if current number is same as previous and previous was not included
            if index > 0 and nums[index] == nums[index - 1] and not prev_included:
                print(f"{indent}🔥 Pruned duplicate nums[{index}] = {nums[index]}")
                return

            # Include current number
            current_subset.append(nums[index])
            print(f"{indent}Include nums[{index}] = {nums[index]}")
            backtrack(index + 1, True, depth + 1)
            current_subset.pop()
            print(f"{indent}Backtrack (removed {nums[index]})")

        backtrack(0, False, 0)
        return all_subsets

# Example Usage
solution = Solution()
result = solution.subsetsWithDup([1, 2, 2])
print("\nFinal Result:", result)
```
---
```
Exclude nums[0] = 1
  Exclude nums[1] = 2
    Exclude nums[2] = 2
      ✅ Adding subset: []
    🔥 Pruned duplicate nums[2] = 2
  Include nums[1] = 2
    Exclude nums[2] = 2
      ✅ Adding subset: [2]
    Include nums[2] = 2
      ✅ Adding subset: [2, 2]
    Backtrack (removed 2)
  Backtrack (removed 2)
Include nums[0] = 1
  Exclude nums[1] = 2
    Exclude nums[2] = 2
      ✅ Adding subset: [1]
    🔥 Pruned duplicate nums[2] = 2
  Include nums[1] = 2
    Exclude nums[2] = 2
      ✅ Adding subset: [1, 2]
    Include nums[2] = 2
      ✅ Adding subset: [1, 2, 2]
    Backtrack (removed 2)
  Backtrack (removed 2)
Backtrack (removed 1)

Final Result: [[], [2], [2, 2], [1], [1, 2], [1, 2, 2]]
```
---
