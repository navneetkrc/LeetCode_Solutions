# 🧠 LeetCode Problem: Contains Duplicate

**Problem Statement**

> Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is **distinct**.

---

## ✅ Approach 1: One-liner using `set()`

```python
def containsDuplicate(nums):
    # Convert the list to a set (which removes duplicates).
    # If the lengths differ, duplicates were present.
    return len(nums) != len(set(nums))
```

### 🔍 Visual Explanation

Input:
`nums = [1, 2, 3, 1]`
→ `set(nums)` = `{1, 2, 3}`
→ `len(nums)` = 4, `len(set(nums))` = 3
→ Return `True`

```
List:       [1, 2, 3, 1]
Set:        {1, 2, 3}      ← Duplicate '1' is removed
Comparison: 4 != 3 → True
```

---

## ✅ Approach 2: Verbose version using `set()` with flag

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        dup_flag = False  # Initially assume no duplicates
        
        if len(nums) > len(set(nums)):
            dup_flag = True  # Found duplicate
        
        return dup_flag
```

### 🔍 Visual Explanation

Input:
`nums = [4, 5, 6, 4]`
→ `set(nums)` = `{4, 5, 6}`
→ Lengths differ → Set flag → Return `True`

```
List:       [4, 5, 6, 4]
Set:        {4, 5, 6}
Flag:       True (duplicate exists)
```

---

## ✅ Approach 3: Manual Hash Set (Basic Hashing)

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()  # Store unique values we've seen so far
        
        for num in nums:
            if num in seen:
                return True  # Found duplicate
            seen.add(num)  # Mark this number as seen
        
        return False  # No duplicates found
```

### 🔍 Visual Explanation

Input:
`nums = [10, 22, 10]`

```
Seen Set = {}

Step 1: 10 not in seen → Add 10 → seen = {10}
Step 2: 22 not in seen → Add 22 → seen = {10, 22}
Step 3: 10 is in seen → Return True ✅
```

---

## ✅ Approach 4: Dictionary-based Counting

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        count_map = {}  # Dictionary to track frequency
        
        for num in nums:
            if num in count_map:
                return True  # Duplicate found
            else:
                count_map[num] = 1  # First occurrence
        
        return False  # No duplicates
```

### 🔍 Visual Explanation

Input:
`nums = [3, 1, 4, 2, 3]`

```
count_map = {}

Step 1: Add 3 → {3:1}
Step 2: Add 1 → {3:1, 1:1}
Step 3: Add 4 → {3:1, 1:1, 4:1}
Step 4: Add 2 → {3:1, 1:1, 4:1, 2:1}
Step 5: 3 already in map → Return True ✅
```

---

## ✅ Approach 5: Using `collections.Counter`

```python
from collections import Counter

def containsDuplicate(nums):
    # Count frequency of all numbers
    # Return True if any number appears more than once
    return any(count > 1 for count in Counter(nums).values())
```

### 🔍 Visual Explanation

Input:
`nums = [2, 5, 1, 2]`
→ `Counter(nums)` = `{2: 2, 5: 1, 1: 1}`
→ Check if any value > 1 → Yes → Return True ✅

```
Counter = {
    2: 2,
    5: 1,
    1: 1
}
→ 2 appears twice → Duplicate found
```

---

## 📊 Summary Comparison

| Approach | Method              | Time Complexity | Space Complexity | Suitable For              |
| -------- | ------------------- | --------------- | ---------------- | ------------------------- |
| 1        | `set()` one-liner   | O(n)            | O(n)             | Quick solutions           |
| 2        | Flag + set          | O(n)            | O(n)             | Teaching beginners        |
| 3        | Manual hash set     | O(n)            | O(n)             | Interviews, understanding |
| 4        | Dictionary counting | O(n)            | O(n)             | When frequency matters    |
| 5        | `Counter`           | O(n)            | O(n)             | Pythonic one-liners       |

---
