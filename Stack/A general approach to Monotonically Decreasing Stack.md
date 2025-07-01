# 📉 Monotonic Decreasing Stack Pattern (Next Greater Problems)

---

## 📌 What’s the Pattern?

A **monotonic decreasing stack** keeps values **decreasing from bottom to top**. As we scan:

* We **pop** while the current element is **greater** than the stack’s top.
* This helps in problems like finding the **next greater element**, **previous greater**, etc.

---

## 🔁 Q1. Next Greater Element to the Right (NGER)

### 🧠 Interview Intent

* LIFO tracking of unprocessed elements
* Rightward comparison using reverse scan

### ✅ Code:

```python
def next_greater_right(nums):
    stack = []
    result = [-1] * len(nums)
    
    for i in reversed(range(len(nums))):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])
    
    return result
```

### 💡 Example:

Input: `[2, 1, 2, 4, 3]` → Output: `[4, 2, 4, -1, -1]`

---

## 🔙 Q2. Next Greater Element to the Left (NGEL)

### 🧠 Interview Intent

* Leftward variant of NGE
* Tests direction-awareness in stack pattern

### ✅ Code:

```python
def next_greater_left(nums):
    stack = []
    result = [-1] * len(nums)

    for i in range(len(nums)):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])
    
    return result
```

---

## 📊 Q3. Stock Span Problem

### 🧠 Interview Intent

* Understanding of monotonic stacks + distances
* Applied to financial domain

### ✅ Code:

```python
def stock_span(prices):
    stack = []
    span = [0] * len(prices)

    for i, price in enumerate(prices):
        while stack and prices[stack[-1]] <= price:
            stack.pop()
        span[i] = i - stack[-1] if stack else i + 1
        stack.append(i)
    
    return span
```

### 💡 Example:

Input: `[100, 80, 60, 70, 60, 75, 85]` → Output: `[1, 1, 1, 2, 1, 4, 6]`

---

## 🌡️ Q4. Daily Temperatures (Leetcode 739)

### 🧠 Interview Intent

* Nearest Greater Element + index difference
* Real-world forecasting analogy

### ✅ Code:

```python
def daily_temperatures(temps):
    stack = []
    result = [0] * len(temps)

    for i, temp in enumerate(temps):
        while stack and temps[stack[-1]] < temp:
            prev = stack.pop()
            result[prev] = i - prev
        stack.append(i)
    
    return result
```

### 💡 Example:

Input: `[73, 74, 75, 71, 69, 72, 76, 73]` → Output: `[1, 1, 4, 2, 1, 1, 0, 0]`

---

## 🔄 Q5. Circular Next Greater Element (Leetcode 503)

### 🧠 Interview Intent

* Cyclic array handling
* Advanced variation of NGER

### ✅ Code:

```python
def next_greater_circular(nums):
    n = len(nums)
    result = [-1] * n
    stack = []

    for i in range(2 * n):
        curr = nums[i % n]
        while stack and nums[stack[-1]] < curr:
            result[stack.pop()] = curr
        if i < n:
            stack.append(i)
    
    return result
```

### 💡 Trick:

Traverse `2 * n` to simulate wrap-around behavior.

---

## 🧠 Pattern Summary Table

| Problem                       | Stack Pattern           | Time | Space |
| ----------------------------- | ----------------------- | ---- | ----- |
| Next Greater to Right (NGER)  | Mono Decreasing, Right  | O(n) | O(n)  |
| Next Greater to Left (NGEL)   | Mono Decreasing, Left   | O(n) | O(n)  |
| Stock Span                    | Mono Decreasing + Index | O(n) | O(n)  |
| Daily Temperatures            | Index + NGER            | O(n) | O(n)  |
| Circular Next Greater Element | Circular Mono Stack     | O(n) | O(n)  |

---

## 🚨 Red Flags

* Forgetting to track indices for span-related problems
* Failing to handle duplicates properly
* Not simulating circular behavior correctly

---

## 💬 Interview Talking Points

* “This is a classic monotonic decreasing stack – we pop until we find the next greater.”
* “We’re storing indices to calculate span/distance later.”
* “We simulate wrap-around by looping twice over the array.”

---
