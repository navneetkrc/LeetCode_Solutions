# 🧱 Stack Interview Prep Guide – Parenthesis Problems

---

## 🧭 Pattern: Parenthesis Matching & Expression Evaluation

### 💡 Core Idea

Use a stack to track state — whether it’s unmatched brackets, operators, or substrings.

---

## ✅ Q1. Valid Parentheses

### 🧠 Interview Intent

Test understanding of:

* Stack fundamentals
* Matching conditions
* Mapping data (e.g., `{':'}'`) for symmetry

### 📝 Step-by-step Walkthrough

**Input:** `s = "({[]})"`
**Stack State:**

| Char | Stack            | Action                     |
| ---- | ---------------- | -------------------------- |
| `(`  | \[`(`]           | Push                       |
| `{`  | \[`(`, `{`]      | Push                       |
| `[`  | \[`(`, `{`, `[`] | Push                       |
| `]`  | \[`(`, `{`]      | Pop `[` and match with `]` |
| `}`  | \[`(`]           | Pop `{` and match with `}` |
| `)`  | \[]              | Pop `(` and match with `)` |

### ⚠️ Common Pitfalls

* Not checking stack emptiness before popping
* Assuming only one bracket type
* Using `if-else` instead of dictionary mapping

### 🔄 Follow-Up Variations

* Return index of the mismatch
* Support Unicode bracket types
* Multiple bracket sets in one string

### 💡 Optimization Hints

* Use a hash map: `closing_to_opening = {')': '(', ']': '[', '}': '{'}`
* Early exit on mismatch

### 💬 Interviewer Notes

* Are they covering empty string as valid?
* Do they check both unmatched opening & closing?

---

## 🧼 Q2. Redundant Parentheses

### 🧠 Interview Intent

Evaluates:

* Stack usage in parsing expressions
* Identifying unnecessary components

### 📝 Step-by-step Walkthrough

**Input:** `"((a+b))"`

| Char | Stack                      | Action                                   |
| ---- | -------------------------- | ---------------------------------------- |
| `(`  | \[`(`]                     | Push                                     |
| `(`  | \[`(`, `(`]                | Push                                     |
| `a`  | \[`(`, `(`, `a`]           | Push                                     |
| `+`  | \[`(`, `(`, `a`, `+`]      | Push                                     |
| `b`  | \[`(`, `(`, `a`, `+`, `b`] | Push                                     |
| `)`  | \[`(`]                     | Pop until `(`; found operator ✅          |
| `)`  | \[]                        | Next `)` hits `(` directly → Redundant ❌ |

### ⚠️ Common Pitfalls

* Not handling expressions without operators
* Popping without checking for `(`

### 🔄 Follow-Up Variations

* Return all indices of redundant parentheses
* Modify string to remove redundant ones

### 💡 Optimization Hints

* Use a flag `has_operator` during popping

---

## ➕ Q3. Minimum Add to Make Parentheses Valid

### 🧠 Interview Intent

Focuses on:

* Understanding balance
* Thinking in terms of counters and not just stack

### 📝 Step-by-step Walkthrough

**Input:** `s = "()))(("`

Track two counters:

* `open_required` for unmatched opening
* `close_required` for unmatched closing

| Char | open\_required | close\_required | Explanation      |
| ---- | -------------- | --------------- | ---------------- |
| `(`  | 1              | 0               | Need one closing |
| `)`  | 0              | 0               | One pair closed  |
| `)`  | 0              | 1               | Extra closing    |
| `)`  | 0              | 2               | Extra closing    |
| `(`  | 1              | 2               | Another opening  |
| `(`  | 2              | 2               | Another opening  |

**Result:** `open_required + close_required = 4`

### ⚠️ Common Pitfalls

* Misusing stack when a simple counter works
* Confusing balance with validity

### 🔄 Follow-Up Variations

* Modify string to make it valid using min additions
* Similar problem with `{}` and `[]`

---

## 🧩 Q4. Longest Valid Parentheses

### 🧠 Interview Intent

Tests:

* Stack with index tracking
* Substring length calculation
* Handling nested and overlapping substrings

### 📝 Step-by-step Walkthrough

**Input:** `s = ")()())"`

| Index | Stack    | Max Length | Action                       |
| ----- | -------- | ---------- | ---------------------------- |
| -1    | \[-1]    | 0          | Sentinel for base index      |
| 0 `)` | \[-1]    | 0          | Invalid; stays as is         |
| 1 `(` | \[-1, 1] | 0          | Push index                   |
| 2 `)` | \[-1]    | 2          | Pop and calculate `2 - (-1)` |
| 3 `(` | \[-1, 3] | 2          | Push index                   |
| 4 `)` | \[-1]    | 4          | Pop and calculate `4 - 1`    |
| 5 `)` | \[]      | 4          | Invalid → reset base to `5`  |

### ⚠️ Common Pitfalls

* Not using index stack
* Forgetting sentinel value `-1`

### 🔄 Follow-Up Variations

* Return actual substring
* Return count of such substrings

---

## 🔄 Q5. Reverse Each Word in a String

### 🧠 Interview Intent

Tests:

* Stack for character reversal
* Word boundary recognition

### 📝 Walkthrough

**Input:** `"Hello World"`
**Output:** `"olleH dlroW"`

Use stack per word:

* Push characters till space
* Pop to reverse word

### ⚠️ Pitfalls

* Losing spaces or multiple spaces
* Not clearing stack per word

---

## 🧮 Q6. Evaluate Reverse Polish Notation

### 🧠 Interview Intent

* Evaluate expression trees
* Operand-operator sequencing
* Stack-based expression evaluation

### 📝 Step-by-step

**Input:** `["2","1","+","3","*"]`
→ `((2 + 1) * 3) = 9`

| Token | Stack   | Action             |
| ----- | ------- | ------------------ |
| `2`   | \[2]    | Push               |
| `1`   | \[2, 1] | Push               |
| `+`   | \[3]    | Pop 2,1 → Push 2+1 |
| `3`   | \[3, 3] | Push               |
| `*`   | \[9]    | Pop 3,3 → Push 3×3 |

### ⚠️ Pitfalls

* Reversing operands during pop
* Handling division rounding

### 🔄 Variants

* Support `^`, `%`, variables
* Return expression string from RPN

---

## 📊 Summary Table

| Problem                           | Time Complexity | Space Complexity | Pattern          |
| --------------------------------- | --------------- | ---------------- | ---------------- |
| Valid Parentheses                 | O(n)            | O(n)             | Bracket Matching |
| Redundant Parentheses             | O(n)            | O(n)             | Expression Stack |
| Min Add to Make Valid Parentheses | O(n)            | O(1)             | Counter Tracking |
| Longest Valid Parentheses         | O(n)            | O(n)             | Index Stack      |
| Reverse Each Word                 | O(n)            | O(n)             | Word Reversal    |
| Evaluate Reverse Polish Notation  | O(n)            | O(n)             | Expression Eval  |

---

## 🧾 Cheat Sheet Summary

* **Always track opening indices or types**
* **Use `-1` index sentinel for problems involving lengths**
* **Use counters for balance-based problems**
* **Reverse Polish = Operand Stack + Operator Evaluation**
* **Visualize dry runs for each stack mutation**

---

## 🚨 Interview Red Flags

* Using stack when a counter is sufficient
* Not explaining why stack is needed
* Confusing order of operand popping
* Ignoring edge cases: `""`, `"("`, `")("`

---

## 💬 Interview Talking Points

* "We use a stack because we need LIFO for nesting/undoing"
* "Index stacks help us compute lengths efficiently"
* "Here’s a dry-run to explain my logic"

