**Leetcode 670: Maximum Swap**. 

This includes:

* Clear **intuition**
* **One-pass greedy strategy**
* **Intuitive Function Calling**
* Visually explained **examples**
* Clean, commented Python code with **descriptive variable names**

---

# 🧠 Leetcode 670 - Maximum Swap (Greedy, One-Pass)

## 📘 Problem Statement

Given a non-negative integer `num`, you are allowed **at most one swap** of **two digits** in the number. Return the maximum possible number you can obtain.

---

### 🧪 Examples:

| Input | Output | Explanation                     |
| ----- | ------ | ------------------------------- |
| 2736  | 7236   | Swap 2 and 7                    |
| 9973  | 9973   | Already maximum, no swap needed |
| 98368 | 98863  | Swap 3 and 8                    |

---

## ✅ Constraints

* `0 <= num <= 10^8`

---

## 🚀 Intuition

We want to **maximize** the number by performing **at most one swap** of two digits. To do this optimally, we use a **greedy strategy**:

### 🔍 Core Idea

> From left to right, find the **first decreasing point** (`valley`). Then, from that point forward, find the **largest digit** to the **right** and swap it with the **leftmost smaller digit**.

---

## 👁️ Visual Example

Let’s walk through: `2736`

```
Index:        0   1   2   3
Digits:       2   7   3   6
               ↑
            First drop (valley) at index 0 (2 < 7)

Right max from index 0: 7 at index 1
Leftmost smaller than 7 = 2 at index 0 → Swap(0,1)

Result = 7236
```

---

## 🧑‍💻 Interview Expectations

* **Greedy optimization** thinking
* Tracking and updating **max value to the right**
* Clean **one-pass solution** with descriptive logic
* Edge case handling (e.g., already max number)

---

## 🧾 Code with Detailed Comments

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        # Convert number to list of digits for easy manipulation
        digits = list(str(num))
        n = len(digits)

        # Step 1: Record the last seen position of each digit (0-9)
        last_seen = {int(d): i for i, d in enumerate(digits)}

        # Step 2: Traverse the digits from left to right
        for i in range(n):
            current_digit = int(digits[i])

            # Try to find a larger digit to swap with (starting from 9 to current_digit + 1)
            for d in range(9, current_digit, -1):
                if d in last_seen and last_seen[d] > i:
                    # Swap and return early since we can only do one swap
                    swap_idx = last_seen[d]
                    digits[i], digits[swap_idx] = digits[swap_idx], digits[i]
                    return int("".join(digits))

        # Already the largest number
        return num
```

---

## 🔄 Alternate Intuitive Approach (Valley + Max Right)

Here’s your original logic, but rewritten for **clarity and explanation**:

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        def find_max_digit_on_right(start_idx: int, digits: list[str]) -> tuple[str, int]:
            """Finds the largest digit from index `start_idx` to end (inclusive),
               preferring the **rightmost** occurrence if duplicates."""
            max_digit = digits[start_idx]
            max_index = start_idx
            for i in range(start_idx, len(digits)):
                if digits[i] >= max_digit:
                    max_digit = digits[i]
                    max_index = i
            return max_digit, max_index

        def find_first_smaller_on_left(target_digit: str, digits: list[str]) -> int:
            """Finds the **leftmost** digit smaller than `target_digit`."""
            for i in range(len(digits)):
                if digits[i] < target_digit:
                    return i
            return -1  # fallback, should not happen

        digits = list(str(num))
        n = len(digits)
        valley_index = -1

        # Step 1: Find the first "valley" — where a digit is smaller than the next one
        for i in range(n - 1):
            if digits[i] < digits[i + 1]:
                valley_index = i
                break

        # Already in descending order, no swap needed
        if valley_index == -1:
            return num

        # Step 2: Find largest digit after the valley
        max_right_digit, max_right_index = find_max_digit_on_right(valley_index, digits)

        # Step 3: Find leftmost digit less than this max_right_digit
        left_swap_index = find_first_smaller_on_left(max_right_digit, digits)

        # Step 4: Swap and return
        digits[left_swap_index], digits[max_right_index] = digits[max_right_index], digits[left_swap_index]
        return int("".join(digits))
```

---

## 🧠 Summary

| Concept               | Description                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| **Greedy**            | Always look for the best swap to maximize the number                        |
| **Right-to-left max** | Helps determine optimal swap                                                |
| **One swap only**     | So we must **swap the first left smaller digit** with the **rightmost max** |
| **Early exit**        | As soon as we find a valid beneficial swap                                  |

---

## 🥇 Final Tips for Interviews

* Clearly explain **why only one swap is needed**
* Highlight **greedy decisions**: swapping the **first left smaller** with **rightmost biggest**
* Code clarity matters: **use meaningful variable names**
* Discuss time complexity: **O(n)** for one pass and digit lookup


---
```python
def maximumSwap(num: int) -> int:
    # Step 1: Convert to list of digits
    digits = list(str(num))
    n = len(digits)
    
    # Step 2: Create next_greater array from right to left
    max_digit = digits[-1]
    next_greater = [''] * n
    next_greater[-1] = max_digit
    
    for i in range(n - 2, -1, -1):
        if digits[i] > max_digit:
            max_digit = digits[i]
        next_greater[i] = max_digit

    # Step 3: Traverse from left to right to find first swap opportunity
    for i in range(n):
        if digits[i] < next_greater[i]:
            target = next_greater[i]
            swap_index_left = i
            
            # Find rightmost index with the target digit
            for j in range(n - 1, -1, -1):
                if digits[j] == target:
                    swap_index_right = j
                    break
            
            # Perform the swap
            digits[swap_index_left], digits[swap_index_right] = digits[swap_index_right], digits[swap_index_left]
            return int(''.join(digits))
    
    # If no swap needed
    return num
    
```
---
