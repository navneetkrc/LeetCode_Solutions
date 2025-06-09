# 🧠 Leetcode 670 - Maximum Swap

*Category: Greedy, One-Pass, Array Manipulation*

---

## 📘 Problem Statement

Given a non-negative integer `num`, you are allowed **at most one swap** of **two digits** in the number. Return the **maximum** possible number you can obtain.

---

### 🧪 Examples:

| Input    | Output   | Explanation                     |
| -------- | -------- | ------------------------------- |
| 2736     | 7236     | Swap 2 and 7                    |
| 9973     | 9973     | Already maximum, no swap needed |
| 98368    | 98863    | Swap 3 and 8                    |
| 10909091 | 90909011 | Swap 1 and 9                    |

---

## ✅ Constraints

* `0 <= num <= 10^8`

---

## 🚀 Intuition

### 🔍 Core Idea

> To maximize the number with **at most one swap**, **swap the first left-side digit** that is **smaller than a larger digit on the right**, with the **rightmost occurrence** of that larger digit.

---

## 🎯 Interview Insight

| Focus Area             | Why It Matters                                   |
| ---------------------- | ------------------------------------------------ |
| **Greedy strategy**    | Optimal decision for single-swap constraint      |
| **Digit manipulation** | Requires treating the number as a list of digits |
| **One-pass logic**     | Optimal time complexity                          |
| **Edge handling**      | Already-max number or repeated digits            |

---

## 👁️ Visual Example: `2736`

```
Index:        0   1   2   3
Digits:       2   7   3   6
              ↑
            First drop at index 0 (2 < 7)

Rightmost max after index 0: 7 at index 1
Swap index 0 and 1 → Result = 7236
```

---

# 🧩 Approach 1: Last Seen Greedy (One-Pass)

> ✅ Best for **interview settings** due to clarity and optimality.

### 🔧 Strategy

1. Traverse the digits from **left to right**
2. For each digit, check if a **bigger digit** appears **later** (track using `last_seen`)
3. If found, **swap** with the **rightmost largest digit**

---

### 💡 Key Detail: Track last positions of all digits (0-9)

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        digits = list(str(num))
        last_seen = {int(d): i for i, d in enumerate(digits)}
        
        for i, digit in enumerate(digits):
            current_digit = int(digit)
            
            for d in range(9, current_digit, -1):
                if d in last_seen and last_seen[d] > i:
                    # Swap and return
                    j = last_seen[d]
                    digits[i], digits[j] = digits[j], digits[i]
                    return int("".join(digits))
        return num
```

### ✅ Time: O(n) | Space: O(1)

---

# 🧭 Approach 2: Valley + Max Right Strategy

> 🏞️ “Valley” = first point where number stops increasing

### 🎓 Explanation Steps

1. Find **first valley point** (where digit\[i] < digit\[i+1])
2. From there, locate the **rightmost largest digit**
3. Find the **leftmost digit** smaller than this max and **swap**

---

### 🧠 Visual: `98368`

```
Index:       0   1   2   3   4
Digits:      9   8   3   6   8

Step 1: valley at index 1 (8 > 3)
Step 2: max right = 8 at index 4
Step 3: find first digit < 8 ⇒ 3 at index 2

Swap 2 and 4 ⇒ Result = 98863
```

---

### 🧾 Code

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        def find_max_digit_on_right(start_idx, digits):
            max_digit, max_index = digits[start_idx], start_idx
            for i in range(start_idx, len(digits)):
                if digits[i] >= max_digit:
                    max_digit, max_index = digits[i], i
            return max_digit, max_index

        def find_first_smaller_on_left(target, digits):
            for i, d in enumerate(digits):
                if d < target:
                    return i
            return -1

        digits = list(str(num))
        valley = next((i for i in range(len(digits)-1) if digits[i] < digits[i+1]), -1)

        if valley == -1:
            return num

        max_d, max_idx = find_max_digit_on_right(valley, digits)
        left_idx = find_first_smaller_on_left(max_d, digits)
        digits[left_idx], digits[max_idx] = digits[max_idx], digits[left_idx]
        return int(''.join(digits))
```

---

# 🌀 Approach 3: `next_greater[]` Array (Scan Right-to-Left)

> Track the **maximum digit to the right of each position**
> Use that to decide swap

---

### 🧪 Example: `19843`

```
digits = [1, 9, 8, 4, 3]
next_greater = [9, 9, 4, 4, 3]

→ digit[0] < next_greater[0]
→ target = 9, swap index 0 and 1 ⇒ Result = 91843
```

---

### 🔧 Code

```python
def maximumSwap(num: int) -> int:
    digits = list(str(num))
    n = len(digits)
    next_greater = [''] * n

    # Step 1: Build next_greater[]
    max_digit = digits[-1]
    for i in range(n - 1, -1, -1):
        if digits[i] > max_digit:
            max_digit = digits[i]
        next_greater[i] = max_digit

    # Step 2: Find swap
    for i in range(n):
        if digits[i] < next_greater[i]:
            target = next_greater[i]
            for j in range(n - 1, i, -1):
                if digits[j] == target:
                    digits[i], digits[j] = digits[j], digits[i]
                    return int(''.join(digits))
    return num
```

---

## 🧠 Summary Table

| Approach                  | Key Idea                                   | Time | Space |
| ------------------------- | ------------------------------------------ | ---- | ----- |
| **1. Greedy + Last Seen** | Swap leftmost small digit w/ rightmost big | O(n) | O(1)  |
| **2. Valley + Max Right** | Find first valley, then max right          | O(n) | O(1)  |
| **3. Right Max Tracking** | Build `next_greater[]`, then decide swap   | O(n) | O(n)  |

---

## 🥇 Interview Tips

* 🔁 **Single swap allowed** ⇒ optimize that one choice
* 🔢 Convert number to **digit list**
* 💡 Use **rightmost occurrence** of max digit
* 🧪 Always test:

  * Already max (e.g., `9876`)
  * Repeated digits (e.g., `1993`)
  * Starting with `0` (e.g., `10909091`)

---

## ✍️ Custom Test Input to Try

```python
inputs = [2736, 9973, 98368, 10909091, 19843]
for num in inputs:
    print(f"Input: {num}, Maximum Swap: {maximumSwap(num)}")
```

---
