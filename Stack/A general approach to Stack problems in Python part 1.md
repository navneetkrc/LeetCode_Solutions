## 🧱 Next Greater Element to the Right (NGER)

---

#### 📌 Problem Statement

Given an array of integers `arr`, for every element, find the **next greater element to its right**. If no such element exists, place `-1`.

**Input:**
`arr = [4, 5, 2, 10, 8]`

**Output:**
`[5, 10, 10, -1, -1]`

---

#### 💡 Intuition Behind Using a Stack

* We traverse the array **from right to left**.
* We maintain a **monotonic decreasing stack** — i.e., elements in the stack are always in descending order from bottom to top.
* At each index, we:

  * **Pop** all elements smaller than or equal to the current element (`arr[i]`)
  * The next element at the top of the stack (if any) is the **next greater**
  * **Push** the current element onto the stack

➡️ This avoids nested loops and solves the problem in **O(n)** time.

---

#### 📊 Dry Run Example (Right to Left)

Input: `[4, 5, 2, 10, 8]`

```
Index:     4    3    2    1    0
Element:   8   10    2    5    4
Stack:    []  [8]  [10] [10] [5]
Answer:  [-1, -1, 10, 10, 5]
```

---

#### 🧠 What’s Being Tested in Interviews

* Understanding of **monotonic stack pattern**
* Ability to convert a **brute-force O(n²)** approach to an **optimal O(n)** one
* Stack operations: **push, pop, peek**
* Traversal direction choice (why right-to-left?)

---

#### ❗ Common Pitfalls

* Traversing **left to right** without adjusting the stack logic
* Not clearing smaller elements (may lead to incorrect answers)
* Forgetting to handle the case where **no greater element exists**

---

#### 🛠️ Python Code (with Comments)

```python
def next_greater_element_right(arr):
    n = len(arr)
    result = [-1] * n  # Default all to -1
    stack = []  # Stack to keep the next greater candidates

    # Traverse from right to left
    for i in range(n - 1, -1, -1):
        current = arr[i]

        # Remove elements that are <= current
        while stack and stack[-1] <= current:
            stack.pop()

        # If stack not empty, top is next greater
        if stack:
            result[i] = stack[-1]

        # Push current element to stack
        stack.append(current)

    return result
```

---

#### ✅ Sample Input / Output

```python
>>> next_greater_element_right([4, 5, 2, 10, 8])
[5, 10, 10, -1, -1]
```

---

#### 🔁 Follow-Up Questions

* Can you do this for a **circular array**? (→ [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/))
* Can you modify this for **Previous Greater Element** or **Next Smaller Element**?
* Can you return the **indices** instead of the elements?

---

#### ❓FAQ Style Notes

| Question                     | Answer                                                              |
| ---------------------------- | ------------------------------------------------------------------- |
| Why use a stack?             | We need to remember potential “next greater” candidates as we move. |
| Why traverse right to left?  | To maintain the stack of "next" elements for each index.            |
| What does the stack contain? | Only values greater than the current being processed.               |

---

#### 🔍 Mistakes to Avoid

* Misplacing the result when no greater element is found
* Not updating the stack correctly (e.g., forgetting to pop)
* Using extra space unnecessarily (you only need one stack + result array)

---

Great! Here's a complete **Markdown-formatted interview prep document** for:

---

## 🔁 Q2) Next Greater Element to the Right – II

**(Leetcode 503: Next Greater Element II)**
**Topic:** Monotonic Stack | Circular Arrays
**Difficulty:** Medium
----------------------

### 📌 Problem Statement

Given a **circular integer array** `nums`, return the **next greater number for every element** in the array.

* The next greater number of a number `x` is the **first greater number** to its **right**, **traversing the array circularly**.
* If it doesn't exist, return `-1`.

#### 🧪 Example:

```python
Input:  nums = [5, 4, 3, 2, 1]
Output: [-1, 5, 5, 5, 5]

Input:  nums = [1, 2, 1]
Output: [2, -1, 2]
```

---

### 💡 Intuition

This is an extension of the **Next Greater Element to the Right (NGER)** problem.
But due to the **circular nature**, each element must look *"beyond the array end"* to its beginning for its next greater.

➡️ So we **simulate two passes** over the array to handle circular behavior.

---

### 🧠 Interviewer's Perspective

* Do you understand the **monotonic stack pattern**?
* Can you handle **circular conditions** efficiently?
* Will you use **modulus indexing** or **duplicate iteration** wisely?
* Do you write **space/time optimal** code?

---

### ⚙️ Approach

* We’ll **simulate 2N traversal** (i.e., loop over the array twice)
* Use `i % n` to wrap around circularly
* Keep a **stack of indices** (not values) to track potential answers
* At each index (in reverse), we:

  * Pop from the stack while the top is less than or equal to current value
  * Top of stack is the next greater element (if stack is not empty)
  * Push current index to the stack

---

### 🛠️ Python Code (Optimal + Commented)

```python
def nextGreaterElements(nums):
    n = len(nums)
    result = [-1] * n  # Initialize with -1 for default
    stack = []  # Stack will store indices

    # Traverse the array twice in reverse (simulate circular)
    for i in range(2 * n - 1, -1, -1):
        index = i % n  # Circular indexing

        # Maintain monotonic decreasing stack
        while stack and nums[stack[-1]] <= nums[index]:
            stack.pop()

        # Assign result only in first pass
        if i < n:
            if stack:
                result[index] = nums[stack[-1]]

        # Push current index onto stack
        stack.append(index)

    return result
```

---

### 🧪 Dry Run Example: `[1, 2, 1]`

```
Stack trace (index | value):

Pass 1 (i = 5 to 3): Stack = [], simulate values from end
Pass 2 (i = 2 to 0):

i=2 -> val=1: stack empty → -1
i=1 -> val=2: stack empty → -1
i=0 -> val=1: stack = [1] → next greater = nums[1] = 2
```

Output: `[2, -1, 2]`

---

### 🧠 What's Being Tested

* Understanding of circular array traversal
* Efficient use of stack with mod (%) operation
* Recognizing a **variation of the NGER pattern**
* Space vs Time trade-offs

---

### ❗ Common Pitfalls

* Forgetting to use `i % n` for circular indexing
* Not simulating the array twice
* Trying to modify the array itself or using extra arrays unnecessarily
* Using values in the stack instead of **indices**

---

### ✅ Sample Inputs

```python
>>> nextGreaterElements([1, 2, 1])
[2, -1, 2]

>>> nextGreaterElements([5, 4, 3, 2, 1])
[-1, 5, 5, 5, 5]

>>> nextGreaterElements([1, 2, 3, 4, 3])
[2, 3, 4, -1, 4]
```

---

### ❓FAQ Style Notes

| Question                            | Answer                                                                 |
| ----------------------------------- | ---------------------------------------------------------------------- |
| Why use `2n` iterations?            | To simulate looking "after the end" in a circular array                |
| Why use a stack of **indices**?     | It allows comparison and also lets us write into `result[i]` correctly |
| When do we assign values to result? | Only during the **first `n` iterations**                               |

---

### ⚠️ Mistakes to Avoid

* Confusing index vs value in stack
* Not using circular behavior correctly with `% n`
* Writing to `result[]` during both passes (should write only in first)

---

### 🔁 Follow-up Questions

* Can you modify this to get the **previous greater element** circularly?
* What if you had to return **distance** to the next greater element?
* What if the array was **doubly circular**, i.e., both directions?

---
Perfect! You're referring to a **variation of the circular NGE** problem where we **pre-fill the stack** by traversing the array once in reverse first — this effectively populates the stack with elements that would appear on the *"left side"* of the array in a circular view.

Below is a **clean, markdown-enhanced document** that follows your approach and includes all the interviewer-focused elements.

---

## 🔄 Q3) Next Greater Element to the Right in a Circular Array (Pre-Fill Stack Variant)

### 🔍 Problem Statement

Given a **circular array** `nums`, return the **Next Greater Element to the Right (NGER)** for each element. If no greater element exists, return `-1`.

---

### 💡 Intuition Behind This Variant

Instead of simulating 2N iterations, we take advantage of **pre-filling the stack** with values **in reverse order** before the main traversal. This means:

* By pushing elements in reverse first, the stack contains "future" values from the left **(due to circularity)**.
* When we traverse normally from left to right, the stack already has what would have been the **rightward circular elements** available.

This is like **rotating the array once in reverse and initializing the stack**.

---

### 📊 Visualization

Input: `[1, 2, 1]`

We **first fill the stack** with values from **right to left**:

```text
Stack after prefill: [1, 2, 1]  (right-to-left)
Now traverse left to right and use this preloaded stack
```

---

### 🛠️ Python Code (with Explanation)

```python
def nextGreaterElements_circular_prefill(nums):
    n = len(nums)
    result = [-1] * n
    stack = []

    # Pre-fill stack from reverse to simulate circularity
    for i in range(n - 1, -1, -1):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        stack.append(nums[i])

    # Now do the actual left-to-right traversal
    for i in range(n):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])  # push current for future elements

    return result
```

---

### ✅ Sample Execution

```python
>>> nextGreaterElements_circular_prefill([1, 2, 1])
[2, -1, 2]

>>> nextGreaterElements_circular_prefill([5, 4, 3, 2, 1])
[-1, 5, 5, 5, 5]
```

---

### 🧠 Interviewer Expectations

* Do you understand **why pre-filling** works in circular arrays?
* Can you explain how this is different from the **2N approach**?
* Can you maintain clean logic with just one traversal after pre-fill?

---

### ✅ What’s Being Tested

| Concept                      | Expected |
| ---------------------------- | -------- |
| Stack for monotonic sequence | ✅        |
| Circular array traversal     | ✅        |
| Preprocessing with stack     | ✅        |
| Avoiding double looping      | ✅        |

---

### ❗ Common Mistakes

* Forgetting to **pre-fill stack properly**
* Mixing **indices vs values** in stack
* Failing to **update result** correctly before pushing current element
* Writing `result[i] = stack[-1]` **before popping smaller elements**

---

### 🔁 Follow-up Questions

* Can you modify this to return **index** instead of value?
* Can you implement it **in-place** (reusing input array)?
* Compare time/space vs the **2-pass approach**

---

### ❓FAQ Style Notes

| Question                                 | Answer                                                                              |
| ---------------------------------------- | ----------------------------------------------------------------------------------- |
| Is this more efficient than 2N loop?     | Slightly better in practice (1.5N instead of 2N), but same complexity.              |
| What does pre-filling the stack achieve? | It simulates future right-side values before starting main traversal.               |
| Is stack storing values or indices?      | This approach uses **values** (not indices) because we don’t need to map positions. |

---

### 📚 Summary

* ✅ This is a clean and intuitive variation that avoids looping `2n` times.
* ⚡ Faster in practice, and easier to reason about with a good dry-run.
* 💬 Great to discuss in interviews to show you understand both **techniques and tradeoffs**.


---

## 🧮 Comparison of 3 Approaches for NGE to the Right

| Approach                              | Type     | Traversal             | Stack Contents | Circular Handling              | Time Complexity | Space Complexity | Pros                             | Cons                             |
| ------------------------------------- | -------- | --------------------- | -------------- | ------------------------------ | --------------- | ---------------- | -------------------------------- | -------------------------------- |
| **1. Standard NGER**                  | Linear   | Right to Left         | Values         | ❌ Not circular                 | `O(n)`          | `O(n)`           | Simple, intuitive                | Doesn’t work for circular arrays |
| **2. Circular NGER (2N Loop)**        | Circular | 2 \* (Right to Left)  | **Indices**    | ✅ Simulates circular using `%` | `O(n)`          | `O(n)`           | Standard and robust for circular | Slightly more logic-heavy        |
| **3. Circular NGER (Pre-fill Stack)** | Circular | 1 Reverse + 1 Forward | **Values**     | ✅ Uses stack pre-fill trick    | `O(n)`          | `O(n)`           | Elegant and often faster         | Harder to explain in a rush      |

---

### ✅ Detailed Breakdown

---

### 🔹 1. Standard NGER (Non-Circular)

```python
# Input: [4, 5, 2, 10, 8]
# Output: [5, 10, 10, -1, -1]
```

* Use a **monotonic decreasing stack**.
* Traverse **right to left**, pushing values and popping smaller ones.
* Efficient for **non-circular arrays only**.

---

### 🔹 2. Circular NGER (Using 2N Traversal)

```python
# Input: [1, 2, 1]
# Output: [2, -1, 2]
```

* Loop from `2n-1` down to `0`, using `i % n` to wrap around.
* Stack stores **indices** to refer back to the original values.
* Standard method for handling circular conditions.

---

### 🔹 3. Circular NGER (Stack Pre-fill Trick)

```python
# Input: [1, 2, 1]
# Output: [2, -1, 2]
```

* First traverse **reverse once** to **pre-fill stack** with future candidates.
* Then traverse **left to right** once, just like regular NGER.
* Elegant and faster in practice; good for interview follow-ups.

---

### 🧠 Interview Tip: When to Use Which?

| Scenario                                | Best Approach                      |
| --------------------------------------- | ---------------------------------- |
| Regular NGE problem                     | **Standard NGER**                  |
| NGE in Circular Array                   | **2N Traversal or Stack Pre-fill** |
| Space Optimization or Readability Focus | **Stack Pre-fill**                 |
| Edge Case Heavy Input                   | **2N Traversal** is more foolproof |

---

### 📌 Summary

* All 3 solve **Next Greater Element to the Right**
* Only Approaches 2 and 3 work with **circular arrays**
* Showcasing **both circular approaches** in an interview demonstrates a solid grasp of stack patterns and algorithmic thinking

---
Great! Here’s a complete, **interview-focused Markdown document** for:

---

## 🔁 Q4) Next Greater Element to the **Left** (NGEL)

> **Pattern:** Monotonic Stack
> **Direction:** Left-to-Right
> **Difficulty:** Easy to Medium
> **Category:** Classic Stack Pattern

---

### 📌 Problem Statement

Given an array `arr`, for each element, find the **next greater element on its left**. If there is none, return `-1`.

#### 🔢 Example:

```python
Input:  [1, 3, 2, 4]
Output: [-1, -1, 3, -1]
```

---

### 💡 Intuition

This is very similar to **Next Greater to the Right**, but now:

* We scan the array **from left to right** (start → end)
* We maintain a **monotonic decreasing stack**
* For each element:

  * We pop all smaller or equal elements from the stack
  * The **top of the stack** is the next greater **to the left**
  * Push the current element onto the stack

✅ No need to reverse the result at the end (unlike some "previous element" problems).

---

### 📊 Dry Run

Input: `[1, 3, 2, 4]`

| Index | Element | Stack          | Next Greater to Left |
| ----- | ------- | -------------- | -------------------- |
| 0     | 1       | \[]            | -1                   |
| 1     | 3       | \[1]           | -1                   |
| 2     | 2       | \[3]           | 3                    |
| 3     | 4       | \[3, 2] → \[ ] | -1                   |

Final Output: `[-1, -1, 3, -1]`

---

### 🧠 Interviewer Expectations

* Do you understand **directional decisions** in monotonic stack problems?
* Can you adapt NGER logic by simply **changing traversal direction**?
* Can you dry-run and **visually simulate** the stack behavior?

---

### 🛠️ Python Code (Clean + Explained)

```python
def next_greater_element_left(arr):
    result = []
    stack = []

    for current in arr:
        # Pop smaller or equal elements from the left
        while stack and stack[-1] <= current:
            stack.pop()

        # If stack is empty, no greater element to left
        if not stack:
            result.append(-1)
        else:
            result.append(stack[-1])

        # Push current element for future comparisons
        stack.append(current)

    return result
```

---

### ✅ Sample Outputs

```python
>>> next_greater_element_left([1, 3, 2, 4])
[-1, -1, 3, -1]

>>> next_greater_element_left([4, 5, 2, 10, 8])
[-1, -1, 5, -1, 10]
```

---

### ✅ What’s Being Tested

| Concept                       | Covered |
| ----------------------------- | ------- |
| Monotonic Stack (Decreasing)  | ✅       |
| Direction-sensitive traversal | ✅       |
| Reuse of core pattern (NGE)   | ✅       |

---

### ❗ Common Mistakes

* Using **right-to-left** logic for a left-side problem
* Forgetting to pop smaller elements from the stack
* Confusing **stack top vs current** in comparisons

---

### ❓FAQ Style Notes

| Question                              | Answer                                         |
| ------------------------------------- | ---------------------------------------------- |
| Can we use a list instead of a stack? | Yes, as long as you use it in LIFO fashion.    |
| Why is result not reversed?           | Because we're moving left to right already.    |
| What’s the space/time complexity?     | `O(n)` time, `O(n)` space (for stack + result) |

---

### 🔁 Follow-Up Questions

* Can you solve **Next Smaller to Left** with the same structure?
* Can you optimize this if you **only care about yes/no** for "greater exists to left"?
* What changes if array is **circular**?

---

### 📚 Summary

| Feature          | Value                                 |
| ---------------- | ------------------------------------- |
| Time Complexity  | `O(n)`                                |
| Space Complexity | `O(n)`                                |
| Pattern          | Monotonic Stack                       |
| Use Case         | Directional max lookup, span problems |

---

## 🔻 Q5) Next Smaller Element to the Right (NSER)

> **Pattern:** Monotonic Stack
> **Direction:** Right-to-Left
> **Category:** Classic Stack Problem
> **Difficulty:** Easy to Medium

---

### 📌 Problem Statement

Given an array `arr`, for each element, find the **next smaller element to its right**. If there is none, return `-1`.

#### 🧪 Example:

```python
Input:  [4, 5, 2, 10, 8]
Output: [2, 2, -1, 8, -1]
```

---

### 💡 Intuition

This is nearly identical to **Next Greater Element to the Right (NGER)**, except now:

* We’re looking for the **next smaller element**
* Use a **monotonic increasing stack** instead of decreasing
* Traverse from **right to left**
* Pop all elements **greater than or equal** to current
* Top of stack will be the next smaller one

---

### 📊 Dry Run: `[4, 5, 2, 10, 8]`

| Index | Element | Stack          | Next Smaller Right |
| ----- | ------- | -------------- | ------------------ |
| 4     | 8       | \[]            | -1                 |
| 3     | 10      | \[8]           | 8                  |
| 2     | 2       | \[10, 8] → \[] | -1                 |
| 1     | 5       | \[2]           | 2                  |
| 0     | 4       | \[5, 2]        | 2                  |

Final Output: `[2, 2, -1, 8, -1]`

---

### 🧠 Interviewer Expectations

* Can you adapt a known pattern (NGER) to **smaller comparison logic**?
* Are you comfortable identifying **monotonic stack direction**?
* Can you explain why stack is increasing/decreasing based on problem type?

---

### 🛠️ Python Code (Commented)

```python
def next_smaller_element_right(arr):
    n = len(arr)
    result = [-1] * n
    stack = []

    # Traverse from right to left
    for i in range(n - 1, -1, -1):
        current = arr[i]

        # Remove all elements >= current
        while stack and stack[-1] >= current:
            stack.pop()

        # Top of stack is the next smaller element
        if stack:
            result[i] = stack[-1]

        # Push current for next comparisons
        stack.append(current)

    return result
```

---

### ✅ Sample Outputs

```python
>>> next_smaller_element_right([4, 5, 2, 10, 8])
[2, 2, -1, 8, -1]

>>> next_smaller_element_right([1, 3, 0, 2, 5])
[0, 0, -1, -1, -1]
```

---

### ✅ What’s Being Tested

| Concept                        | Covered |
| ------------------------------ | ------- |
| Monotonic Stack (Increasing)   | ✅       |
| Right-to-Left traversal        | ✅       |
| Modifying comparison condition | ✅       |
| Directional reasoning          | ✅       |

---

### ❗ Common Mistakes

* Using `<=` instead of `>=` in the comparison
* Confusing “greater” with “smaller” logic
* Forgetting to traverse from **right to left**

---

### ❓FAQ Style Notes

| Question                    | Answer                                                    |
| --------------------------- | --------------------------------------------------------- |
| Why traverse right to left? | We’re building next element **to the right** of each item |
| Can I use left-to-right?    | Yes, but logic gets more complex                          |
| Space/Time Complexity?      | `O(n)` time, `O(n)` space                                 |

---

### 🔁 Follow-Up Questions

* Can you do this for a **circular array**?
* What changes for **Next Smaller to the Left**?
* How would you return the **distance** instead of the value?

---

### 📚 Summary

| Feature          | Value                        |
| ---------------- | ---------------------------- |
| Time Complexity  | `O(n)`                       |
| Space Complexity | `O(n)`                       |
| Pattern          | Monotonic Stack (Increasing) |
| Use Case         | Nearest lower value to right |


---

## 🔶 Q6: Next Smaller Element to the Left (NSEL)

---

### 🧠 **Interview Intent**

* Tests understanding of **monotonic stack patterns**.
* Evaluates candidate’s ability to adapt a known pattern (`Next Greater Element`) to a new variation by changing traversal and comparison direction.
* Assesses familiarity with efficient **O(n)** solutions over nested loops.

---

### 🔧 Problem Statement

Given an array, find for each element the **first smaller element on the left**.
If none exists, return `-1` for that position.

---

### 📝 **Step-by-Step Walkthrough with Stack Visualization**

**Example**:
`Input: [4, 5, 2, 10, 8]`
`Output: [-1, 4, -1, 2, 2]`

**Dry Run:**

| i | A\[i] | Stack   | Action                          | Result             |
| - | ----- | ------- | ------------------------------- | ------------------ |
| 0 | 4     | \[]     | Stack empty → append -1         | \[-1]              |
| 1 | 5     | \[4]    | 4 < 5 → append 4                | \[-1, 4]           |
| 2 | 2     | \[4, 5] | Pop 5, pop 4 → Stack empty → -1 | \[-1, 4, -1]       |
| 3 | 10    | \[]     | Stack empty → append -1         | \[-1, 4, -1, 2]    |
| 4 | 8     | \[10]   | 10 > 8 → pop 10 → append 2      | \[-1, 4, -1, 2, 2] |

Final Stack state is used to resolve previous smaller values.

---

### ⚠️ **Common Pitfalls**

* Confusing **“left” vs “right”** direction → traverse from start to end.
* Forgetting to pop until stack is valid (`stack[-1] < A[i]`)
* Returning index instead of value
* Reversing the output (not needed for NSEL unlike NSER)

---

### 💻 **Template Code (Python)**

```python
def next_smaller_to_left(arr):
    stack = []
    result = []

    for num in arr:
        while stack and stack[-1] >= num:
            stack.pop()
        result.append(stack[-1] if stack else -1)
        stack.append(num)

    return result
```

---

### 💡 **Optimization Hints & Real-World Analogy**

* Real-world analogy: Think of people in a queue checking who’s shorter behind them. The stack holds potential “smaller” people for quick access.
* Time Complexity: **O(n)** — each element pushed and popped once.
* Space Complexity: **O(n)** for result + stack.

---

### 🔄 **Follow-up Variations**

* **Next Smaller Element to Right (NSER)** → Traverse from end, reverse result.
* **Next Greater Element to Left (NGEL)**
* **Index instead of value** → return index instead of value.
* **Distance to next smaller** instead of value.

---

### 👀 **Red Flags**

* Using nested loops → **O(n²)** solution
* Pushing indices/values inconsistently
* Mishandling empty stack condition
* Modifying input array without copying

---

### 💬 **Interviewer Perspective**

* Is the candidate able to **recognize monotonic stack pattern**?
* Can they correctly **adapt traversal direction**?
* Are they using a clean and reusable **template**?

---

### 🧾 **Cheat Sheet Add-on**

| Variant | Traverse Direction | Comparison    | Final Output |
| ------- | ------------------ | ------------- | ------------ |
| NSEL    | Left → Right       | `<` (smaller) | As-is        |
| NSER    | Right → Left       | `<` (smaller) | Reverse      |
| NGEL    | Left → Right       | `>` (greater) | As-is        |
| NGER    | Right → Left       | `>` (greater) | Reverse      |

---

---

# 📋 Stack-Based Interview Prep: Summary Table (Q1–Q6)

| Q# | Problem Name                                        | Traverse Dir.                 | Comparison    | Stack Maintains          | Output Order | Interview Focus                                              |
| -- | --------------------------------------------------- | ----------------------------- | ------------- | ------------------------ | ------------ | ------------------------------------------------------------ |
| Q1 | **Next Greater Element to the Right (NGER)**        | 🔄 Right → Left               | `>` (greater) | Monotonic **decreasing** | 🔁 Reverse   | Pattern recognition: Monotonic stack, delayed decision       |
| Q2 | **Next Greater Element to the Right - II (NGER-2)** | 🔄 Right → Left (2 passes)    | `>` (greater) | Monotonic **decreasing** | 🔁 Reverse   | Adapting for circular array; modular indexing, edge handling |
| Q3 | **Next Greater in Circular Array**                  | 🔄 Right → Left (simulate 2n) | `>` (greater) | Monotonic **decreasing** | 🔁 Reverse   | Handling circular logic; time-space tradeoffs                |
| Q4 | **Next Greater to the Left (NGEL)**                 | 🔄 Left → Right               | `>` (greater) | Monotonic **decreasing** | ✅ As-is      | Direction switch; mirror logic from NGER                     |
| Q5 | **Next Smaller Element to the Right (NSER)**        | 🔄 Right → Left               | `<` (smaller) | Monotonic **increasing** | 🔁 Reverse   | Subtle sign flip; test understanding of monotonic variant    |
| Q6 | **Next Smaller Element to the Left (NSEL)**         | 🔄 Left → Right               | `<` (smaller) | Monotonic **increasing** | ✅ As-is      | Recognize 4-way pattern; test generalization across problems |

---

## 🧠 Key Interview Insights

* **Traversal direction + comparison sign** determine all variants.
* Interviewers want to see if you can **recognize patterns** and **generalize** them.
* Reversal of output only needed when going from **right to left**.
* Maintaining the correct type of **monotonic stack** is critical:

  * `>` → Monotonic decreasing stack
  * `<` → Monotonic increasing stack
* Expect to be asked:

  * “What changes if it’s circular?”
  * “Can you solve this using a queue instead?”
  * “What’s the space optimization?”

---

## 💬 Talking Point Cheat Code

**“There are only 4 core combinations. Once you know how to flip traversal direction and comparison operator, you can solve any ‘Next Greater/Smaller’ problem confidently. All you need is a monotonic stack and a clear idea of when to reverse the result.”**

---


