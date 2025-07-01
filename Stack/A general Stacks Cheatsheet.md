# 🧠📋 Stack Interview Prep – Definitive Cheat Sheet Summary

---

## 🔢 Parentheses & Expression Parsing

| Problem                         | Pattern             | Time | Space | Key Insight / Talking Point   |
| ------------------------------- | ------------------- | ---- | ----- | ----------------------------- |
| ✅ Valid Parentheses             | Stack matching      | O(n) | O(n)  | LIFO pairing using hash map   |
| 🧼 Redundant Parentheses        | Stack + operators   | O(n) | O(n)  | Look for brackets without ops |
| ➕ Min Add to Make Valid         | Counter (not stack) | O(n) | O(1)  | Count unbalanced `(` and `)`  |
| 🔍 Longest Valid Parentheses    | Index stack         | O(n) | O(n)  | Use `-1` sentinel for base    |
| 🔁 Reverse Each Word            | Stack per word      | O(n) | O(n)  | Clear stack after each word   |
| 🧮 Reverse Polish Notation Eval | Operand stack       | O(n) | O(n)  | Pop 2 operands per operator   |

---

## 📈 Monotonic Increasing Stack

| Problem                           | Pattern               | Time  | Space | Use Case                      |
| --------------------------------- | --------------------- | ----- | ----- | ----------------------------- |
| 🔻 Next Smaller to Right (NSER)   | Mono-Incr, right scan | O(n)  | O(n)  | Pop ≥ current, right boundary |
| 🔻 Next Smaller to Left (NSEL)    | Mono-Incr, left scan  | O(n)  | O(n)  | Pop ≥ current, left boundary  |
| 📊 Largest Rectangle in Histogram | NSER + NSEL by index  | O(n)  | O(n)  | Height × Width with indices   |
| 🧱 Max Area Rectangle (Matrix)    | Histogram per row     | O(mn) | O(n)  | Row-wise histogram building   |
| 🔬 Sum of Subarray Minimums       | Count via monotonic   | O(n)  | O(n)  | `(Left×Right) * A[i]` count   |

---

## 📉 Monotonic Decreasing Stack

| Problem                        | Pattern               | Time | Space | Key Insight                   |
| ------------------------------ | --------------------- | ---- | ----- | ----------------------------- |
| ➕ Next Greater to Right (NGER) | Mono-Decr, right scan | O(n) | O(n)  | Pop ≤ current, look right     |
| ➕ Next Greater to Left (NGEL)  | Mono-Decr, left scan  | O(n) | O(n)  | Pop ≤ current, look left      |
| 💹 Stock Span                  | Mono-Decr + distance  | O(n) | O(n)  | Span = i - prev\_greater\_idx |
| 🌡️ Daily Temperatures         | Index stack + diff    | O(n) | O(n)  | i - prev\_index of < temp     |
| 🔁 Circular NGE                | 2-pass mono-decr      | O(n) | O(n)  | Loop `2n` for wraparound      |

---

## 🎨 Visual Pattern Aids

### ✅ Parentheses Matching

```
s = "{[()]}"
stack = []

Push opening:
stack → ['{', '[', '(']

Match closing:
stack → pop '(', then ']', then '}'
```

---

### 📈 Monotonic Increasing Stack (NSER)

For input: `[5, 3, 8, 1, 2]`

```
Traverse right to left
Stack: Keep increasing values from top to bottom
Pop when current < top
```

---

### 📉 Monotonic Decreasing Stack (NGER)

For input: `[2, 4, 1, 3]`

```
Traverse right to left
Stack: Keep decreasing values from top to bottom
Pop when current > top
```

---

### 🧱 Largest Rectangle Histogram

```
Use indices stack
For height[i], calculate:
    left boundary = prev_smaller
    right boundary = next_smaller
Area = height × (right - left - 1)
```

---

## 🧠 Interview Power Tips

### ✅ Talking Points

* “I’m using a monotonic stack to find the next smaller/greater in O(n) time.”
* “Using index stack helps me track width, not just values.”
* “The problem maps to histogram height updates, reused row-wise.”

### ❌ Red Flags to Avoid

* Not handling edge values (`-1` sentinel for parentheses/length)
* Confusing left vs right variants (NGEL ≠ NGER)
* Failing to reset/flush the stack at key points (like end of array)

---

## ⏱️ Time Management Tips

| Scenario                      | Suggested Strategy            |
| ----------------------------- | ----------------------------- |
| Bracket Matching              | Use dictionary, fast checks   |
| Next Greater/Smaller Problems | Stick to monotonic pattern    |
| Histogram/Max Area            | Focus on width calc via index |
| Unclear Variant (NGR vs NSL)  | Dry-run on small array        |

---

## 🧾 Suggested Prep Order

1. **Easy**: Valid Parentheses, NGR, NSR
2. **Medium**: Histogram Area, Redundant Brackets, RPN
3. **Hard**: Max Area Matrix, Circular NGE, Subarray Minimum Sum

