# 🔍 496. Next Greater Element I – Complete Interview Guide

**Difficulty:** Easy  
**Tags:** Stack, HashMap, Monotonic Stack  
**Asked by:** Amazon, Google, Bloomberg, Microsoft  

---

## 🧩 Problem Statement

You are given two **distinct** 0-indexed integer arrays:

- `nums1` – a **subset** of `nums2`
- `nums2` – a **superset** array

Your task is to find the **next greater element** for each element in `nums1` **as they appear in** `nums2`.

> **Next Greater Element:**  
> The first number to the right of a number `x` in `nums2` that is **greater than `x`**.  
> If no such number exists, return `-1`.

---

## 💡 Example Walkthrough

### Example 1:

```python
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]
````

**Output:** `[-1, 3, -1]`

* `4` → no greater to its right → `-1`
* `1` → next greater is `3`
* `2` → no greater → `-1`

---

## ✅ Constraints

* `1 <= nums1.length <= nums2.length <= 1000`
* `0 <= nums1[i], nums2[i] <= 10^4`
* All values in `nums1` and `nums2` are unique
* `nums1` is a subset of `nums2`

---

## ⚙️ Optimal Approach: Monotonic Stack + Hash Map

### Intuition:

To efficiently find the next greater element in linear time, we use a **monotonic decreasing stack** while scanning `nums2`. This lets us know the **next greater** for every element.

### Time & Space Complexity:

* **Time:** `O(M + N)` → one pass for each array
* **Space:** `O(N)` for the stack and map

---

## 👁️ Visual Intuition: Stack Simulation

Let’s walk through this example:

```python
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]
```

We initialize:

```text
Stack = []
Map = {}
```

### ➤ Step-by-step Stack Simulation

#### 🔹 Element: 1

```
Stack: [1]
Map:   {}
```

#### 🔹 Element: 3

3 > 1 → pop 1, map `1 → 3`

```
Stack: [3]
Map:   {1: 3}
```

#### 🔹 Element: 4

4 > 3 → pop 3, map `3 → 4`

```
Stack: [4]
Map:   {1: 3, 3: 4}
```

#### 🔹 Element: 2

2 < 4 → just push

```
Stack: [4, 2]
Map:   {1: 3, 3: 4}
```

#### 🔚 Post-processing:

Pop remaining elements and map to `-1`:

```
Stack: []
Map:   {1: 3, 3: 4, 4: -1, 2: -1}
```

### ➤ Final Lookup:

For `nums1 = [4, 1, 2]`:

```text
4 → -1  
1 → 3  
2 → -1
```

✅ Final Output: `[-1, 3, -1]`

---

## 🧠 Interview Tips

| 💬 Interviewer Expects You To... | Explanation                                    |
| -------------------------------- | ---------------------------------------------- |
| Recognize brute-force is O(N²)   | Looping for each `nums1[i]` in `nums2`         |
| Optimize with Monotonic Stack    | Realize stack helps in "next greater" problems |
| Use hashmap for quick lookup     | Preprocess once, answer many queries           |
| Explain with an example          | Talk through stack operations visually         |
| Know tradeoffs                   | Extra space but linear time                    |

---

## 🧪 Python Code – Clean, Readable, Interview Ready

```python
from typing import List

class Solution:
    def nextGreaterElement(self, subset_nums: List[int], full_nums: List[int]) -> List[int]:
        # Map to store the next greater element for each number in full_nums
        next_greater_map = {}
        # Stack to maintain decreasing sequence
        decreasing_stack = []

        # Traverse full_nums to build mapping
        for current in full_nums:
            # Resolve all smaller elements waiting for a greater value
            while decreasing_stack and current > decreasing_stack[-1]:
                smaller = decreasing_stack.pop()
                next_greater_map[smaller] = current
            decreasing_stack.append(current)

        # Assign -1 to elements with no next greater
        while decreasing_stack:
            next_greater_map[decreasing_stack.pop()] = -1

        # Build result for subset_nums using the map
        return [next_greater_map[num] for num in subset_nums]
```

---

## 🧾 Sample Test Case

```python
nums1 = [2, 4]
nums2 = [1, 2, 3, 4]
# Output: [3, -1]
```

---

## 📌 Key Takeaways

* 💡 Use **monotonic stacks** for "next greater" problems
* 💡 Use a **map** to decouple preprocessing from querying
* 💡 Think through **state changes** with stack visually
* 💡 Always explain both time and space complexity clearly

---

## 📘 Bonus: Real-life Analogy

Think of `nums2` as a list of students in a line.
Each student wants to know:
**“Who is the first taller student standing ahead of me?”**

You keep track of unresolved students in a **stack**.
Once a taller student comes in, you resolve all previous shorter ones.

---
