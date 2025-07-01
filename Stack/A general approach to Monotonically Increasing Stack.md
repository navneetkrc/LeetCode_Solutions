# 🧱 Stack Interview Prep Guide – Monotonic Increasing Stack

---

## 📌 Pattern: Monotonic Increasing Stack

### 🧠 What It Means

A **monotonic increasing stack** keeps elements in increasing order (from bottom to top). As we traverse:

* We **pop** when the current element is **smaller** than the top.
* Use it to find **previous smaller**, **next smaller**, etc.

---

## 🔢 Q1. Next Smaller Element to the Right (NSER)

### 🧠 Interview Intent

* Stack to process elements in reverse
* Understand LIFO and nearest conditions

### 📝 Dry Run (Input: `[4, 5, 2, 10, 8]` → Output: `[2, 2, -1, 8, -1]`)

### ✅ Code:

```python
def next_smaller_right(nums):
    stack = []
    result = [-1] * len(nums)
    
    for i in reversed(range(len(nums))):
        while stack and stack[-1] >= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])
    
    return result
```

### 💬 Key Variables:

* `stack`: holds potential smaller elements
* `result[i]`: stores the answer for index `i`

---

## 🔙 Q2. Next Smaller Element to the Left (NSEL)

### 🧠 Interview Intent

* Left traversal with monotonic condition
* Real-time minimum tracking

### ✅ Code:

```python
def next_smaller_left(nums):
    stack = []
    result = [-1] * len(nums)

    for i in range(len(nums)):
        while stack and stack[-1] >= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])
    
    return result
```

---

## 🧱 Q3. Largest Rectangle in Histogram

### 🧠 Interview Intent

* Application of NSEL + NSER
* Tricky width calculation with index stacks

### ✅ Code:

```python
def largest_rectangle_area(heights):
    n = len(heights)
    stack = []
    max_area = 0

    for i in range(n + 1):
        current_height = 0 if i == n else heights[i]
        while stack and current_height < heights[stack[-1]]:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    
    return max_area
```

### 📝 Explanation:

* We append index `i` so that width can be computed via positions.
* Sentinel (i == n) ensures final flush of stack.

---

## 🧾 Q4. Maximal Rectangle in a Binary Matrix

### 🧠 Interview Intent

* Reduction to histogram problem
* Multi-pass stack application

### ✅ Code:

```python
def maximal_rectangle(matrix):
    if not matrix: return 0
    cols = len(matrix[0])
    height = [0] * cols
    max_area = 0

    for row in matrix:
        for i in range(cols):
            height[i] = height[i] + 1 if row[i] == '1' else 0
        max_area = max(max_area, largest_rectangle_area(height))
    
    return max_area
```

### 🔁 Uses previous problem as helper.

---

## 📈 Q5. Sum of Subarray Minimums

### 🧠 Interview Intent

* Monotonic stack with index-based distance tracking
* Math + Stack synergy

### ✅ Code:

```python
def sum_subarray_mins(arr):
    MOD = 10**9 + 7
    stack = []
    result = 0

    for i in range(len(arr) + 1):
        curr = 0 if i == len(arr) else arr[i]
        while stack and (i == len(arr) or arr[stack[-1]] >= curr):
            mid = stack.pop()
            left = stack[-1] if stack else -1
            count = (mid - left) * (i - mid)
            result += arr[mid] * count
        stack.append(i)
    
    return result % MOD
```

### 🧮 Logic:

* Each element is the **minimum** for `(mid-left) * (right-mid)` subarrays.

---

## 🧠 Pattern Summary Table

| Problem                      | Stack Pattern          | Time    | Space |
| ---------------------------- | ---------------------- | ------- | ----- |
| Next Smaller Element (Right) | Mono Increasing, Right | O(n)    | O(n)  |
| Next Smaller Element (Left)  | Mono Increasing, Left  | O(n)    | O(n)  |
| Largest Rectangle Histogram  | Index Stack            | O(n)    | O(n)  |
| Max Rectangle Binary Matrix  | Stack + Histogram      | O(n\*m) | O(n)  |
| Sum of Subarray Minimums     | Stack + Math           | O(n)    | O(n)  |

---

## 🚨 Red Flags

* Using value instead of index in histograms
* Confusing NSER with NGER
* Forgetting to flush stack (e.g., at end of iteration)

---

## 💬 Talking Points

* “The stack stores increasing values because we want to find the next smaller.”
* “By keeping indices, we can calculate width between left and right boundaries.”
* “This is an amortized O(n) process — each element enters and exits stack once.”

---
