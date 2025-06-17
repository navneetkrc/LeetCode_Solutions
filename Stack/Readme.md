# Stack Interview Questions – Greg Hogg Playlist

---

## 📚 Table of Contents

1. [Baseball Game](#1-baseball-game)
2. [Valid Parentheses](#2-valid-parentheses)
3. [Daily Temperatures](#3-daily-temperatures)
4. [Evaluate Reverse Polish Notation](#4-evaluate-reverse-polish-notation)
5. [Min Stack](#5-min-stack)
6. [Largest Rectangle in Histogram](#6-largest-rectangle-in-histogram)
7. [Visual Summary Table](#7-visual-summary-table)
8. [Interview Tips & Expectations](#8-interview-tips--expectations)

---

## 1. Baseball Game

- **Description:** Simulate a baseball game with operations that add, remove, or modify scores using a stack to track valid rounds.
- **LeetCode:** [LeetCode 682](https://leetcode.com/problems/baseball-game/)
- **Companies:** Amazon, Google, Facebook[1].

---

## 2. Valid Parentheses

- **Description:** Determine if a string of brackets is valid by checking for matching open/close pairs using a stack.
- **LeetCode:** [LeetCode 20](https://leetcode.com/problems/valid-parentheses/)
- **Companies:** Facebook, Amazon, Microsoft[1].

---

## 3. Daily Temperatures

- **Description:** For each day, find how many days until a warmer temperature using a monotonic stack for efficient lookups.
- **LeetCode:** [LeetCode 739](https://leetcode.com/problems/daily-temperatures/)
- **Companies:** Amazon, Google, Microsoft[1].

---

## 4. Evaluate Reverse Polish Notation

- **Description:** Evaluate arithmetic expressions in Reverse Polish Notation (postfix) using a stack for operand management.
- **LeetCode:** [LeetCode 150](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
- **Companies:** Amazon, Google, Bloomberg[1].

---

## 5. Min Stack

- **Description:** Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
- **LeetCode:** [LeetCode 155](https://leetcode.com/problems/min-stack/)
- **Companies:** Amazon, Google, Microsoft[1].

---

## 6. Largest Rectangle in Histogram

- **Description:** Find the largest rectangle area in a histogram using a stack to maintain indices of increasing heights.
- **LeetCode:** [LeetCode 84](https://leetcode.com/problems/largest-rectangle-in-histogram/)
- **Companies:** Google, Amazon, Facebook[1].

---

## Visual Summary Table

| Problem                          | LeetCode Link                                                                    | Companies                         |
|-----------------------------------|----------------------------------------------------------------------------------|-----------------------------------|
| Baseball Game                    | [LeetCode 682](https://leetcode.com/problems/baseball-game/)                     | Amazon, Google, Facebook          |
| Valid Parentheses                 | [LeetCode 20](https://leetcode.com/problems/valid-parentheses/)                  | Facebook, Amazon, Microsoft       |
| Daily Temperatures                | [LeetCode 739](https://leetcode.com/problems/daily-temperatures/)                | Amazon, Google, Microsoft         |
| Evaluate Reverse Polish Notation  | [LeetCode 150](https://leetcode.com/problems/evaluate-reverse-polish-notation/)  | Amazon, Google, Bloomberg         |
| Min Stack                         | [LeetCode 155](https://leetcode.com/problems/min-stack/)                         | Amazon, Google, Microsoft         |
| Largest Rectangle in Histogram    | [LeetCode 84](https://leetcode.com/problems/largest-rectangle-in-histogram/)     | Google, Amazon, Facebook          |

---

## Stack Interview Tips

- **When to Use Stacks:** Use stacks for problems involving matching pairs, parsing expressions, and tracking previous elements.
- **Common Pitfalls:** Be careful with stack underflow and ensure all elements are processed.
- **Stack Operations:** Most stack operations (push, pop, peek) are O(1).
- **Practice:** Focus on problems involving parentheses matching, monotonic stacks, and expression evaluation[1].

---

## Interview Tips & Expectations

### ✅ Key Expectations

* Walk through **both brute force and optimal** clearly.
* Be able to **simulate stack changes** step-by-step.
* **Explain intuition** behind using stacks: matching, memory, undoing, etc.
* Always **discuss edge cases**: empty input, invalid formats.

### 💡 Advanced Tips

* **Monotonic Stacks:** Often used in sliding window or range queries.
* **Double Stack Approaches:** Useful in problems like *Min Stack*.
* **Expression Parsing:** Understand operator precedence and associativity.
* **Avoid Stack Overflow:** Use iterative where recursion causes depth issues.

---
Approaches
---
## 1. Baseball Game

* **Description:** Simulate baseball score tracking with stack operations.
* **LeetCode:** [682](https://leetcode.com/problems/baseball-game/)
* **Companies:** Amazon, Google, Facebook, Walmart

### 🔍 Brute Force:

Iterate and maintain a list. Update manually.

### 💡 Optimal Approach:

Use a stack to simulate each round with O(n) time.

```python
def calPoints(operations):
    stack = []
    for op in operations:
        if op == "+":
            stack.append(stack[-1] + stack[-2])
        elif op == "D":
            stack.append(2 * stack[-1])
        elif op == "C":
            stack.pop()
        else:
            stack.append(int(op))
    return sum(stack)
```

✅ **Expectation:** Explain all ops clearly, simulate on small input.

---

## 2. Valid Parentheses

* **Description:** Validate bracket matching using a stack.
* **LeetCode:** [20](https://leetcode.com/problems/valid-parentheses/)
* **Companies:** Facebook, Amazon, Microsoft, Adobe

### 🔍 Brute Force:

Check all combinations recursively – expensive.

### 💡 Optimal Approach:

Use a stack to check matches in one pass.

```python
def isValid(s):
    stack = []
    mapping = {')': '(', ']': '[', '}': '{'}
    for char in s:
        if char in mapping.values():
            stack.append(char)
        elif char in mapping:
            if not stack or mapping[char] != stack.pop():
                return False
    return not stack
```

✅ **Expectation:** Handle edge cases: "", single bracket.

---

## 3. Daily Temperatures

* **Description:** For each temperature, find next warmer day.
* **LeetCode:** [739](https://leetcode.com/problems/daily-temperatures/)
* **Companies:** Amazon, Google, Microsoft, Salesforce

### 🔍 Brute Force:

Nested loop check for next warmer day → O(n²)

### 💡 Optimal Approach:

Use a monotonic decreasing stack → O(n)

```python
def dailyTemperatures(temperatures):
    res = [0] * len(temperatures)
    stack = []
    for i, temp in enumerate(temperatures):
        while stack and temperatures[stack[-1]] < temp:
            last = stack.pop()
            res[last] = i - last
        stack.append(i)
    return res
```

✅ **Expectation:** Mention monotonic stack concept.

---

## 4. Evaluate Reverse Polish Notation

* **Description:** Evaluate postfix expressions using stack.
* **LeetCode:** [150](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
* **Companies:** Amazon, Google, Bloomberg, SAP

### 🔍 Brute Force:

Convert to infix and evaluate – not ideal.

### 💡 Optimal Approach:

Use stack to evaluate on the fly.

```python
def evalRPN(tokens):
    stack = []
    for token in tokens:
        if token in "+-*/":
            b, a = stack.pop(), stack.pop()
            if token == '+': stack.append(a + b)
            elif token == '-': stack.append(a - b)
            elif token == '*': stack.append(a * b)
            else: stack.append(int(a / b))
        else:
            stack.append(int(token))
    return stack[0]
```

✅ **Expectation:** Handle int division and precedence.

---

## 5. Min Stack

* **Description:** Stack with constant-time min retrieval.
* **LeetCode:** [155](https://leetcode.com/problems/min-stack/)
* **Companies:** Amazon, Google, Microsoft, Apple

### 🔍 Brute Force:

Scan entire stack to get min → O(n)

### 💡 Optimal Approach:

Maintain auxiliary stack for minimums.

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self):
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self):
        return self.stack[-1]

    def getMin(self):
        return self.min_stack[-1]
```

✅ **Expectation:** Explain auxiliary stack usage.

---

## 6. Largest Rectangle in Histogram

* **Description:** Find the largest rectangle in histogram bars.
* **LeetCode:** [84](https://leetcode.com/problems/largest-rectangle-in-histogram/)
* **Companies:** Google, Amazon, Facebook, Adobe

### 🔍 Brute Force:

Check all pairs of bars for max area → O(n²)

### 💡 Optimal Approach:

Use monotonic stack for left/right boundaries → O(n)

```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    heights.append(0)
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    return max_area
```

✅ **Expectation:** Draw the histogram, walk through steps.

---

