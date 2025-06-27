# 🧮 150. Evaluate Reverse Polish Notation – Complete Interview Guide

**Difficulty:** Medium  
**Tags:** Stack, Math  
**Asked By:** Amazon, Google, Facebook, Microsoft

---

## 📘 Problem Statement

You are given an array of strings `tokens` that represents an **arithmetic expression** in **Reverse Polish Notation** (RPN).

Your task is to **evaluate the expression** and return its result as an integer.

### 📌 Reverse Polish Notation (Postfix):

- Operators follow their operands.  
- You evaluate left-to-right, applying operators to the previous two numbers.

---

### 🎯 Requirements

- Valid operators: `"+"`, `"-"`, `"*"`, `"/"`
- Integer division **truncates toward zero**
- No division by zero
- Expression is always valid
- Each intermediate calculation fits in 32-bit signed int

---

## 🔢 Examples

### Example 1
```text
Input:  tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
````

### Example 2

```text
Input:  tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

### Example 3

```text
Input:  tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
```

---

## 🧠 Intuition

To evaluate a Reverse Polish expression:

1. Use a **stack** to keep track of operands.
2. When you see a number, push it to the stack.
3. When you see an operator:

   * Pop the **top two operands**
   * Apply the operator
   * Push the result back onto the stack

> You always apply the operator as: `left_operand <operator> right_operand`

---

## 👨‍💻 Python Code (Clean and Interview-Ready)

```python
import operator
from typing import List

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        # Map operator strings to corresponding Python functions
        operator_map = {
            '+': operator.add,
            '-': operator.sub,
            '*': operator.mul,
            '/': lambda x, y: int(operator.truediv(x, y))  # Truncate toward zero
        }

        stack = []

        for token in tokens:
            if token not in operator_map:
                # Token is a number → convert and push to stack
                stack.append(int(token))
            else:
                # Token is an operator → pop two operands
                right_operand = stack.pop()
                left_operand = stack.pop()

                # Perform operation and push result
                operation = operator_map[token]
                result = operation(left_operand, right_operand)
                stack.append(result)

        # Final result will be the only value in the stack
        return stack[0]
```

---

## 🔍 Step-by-Step Dry Run (Example 3)

```text
Input: ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]

Stack trace:
[10]
[10, 6]
[10, 6, 9]
[10, 6, 9, 3]
[10, 6, 12]          # 9 + 3
[10, 6, 12, -11]
[10, 6, -132]        # 12 * -11
[10, 0]              # 6 / -132 = 0 (truncated toward zero)
[0]                  # 10 * 0
[0, 17]
[17]                 # 0 + 17
[17, 5]
[22]                 # 17 + 5

✅ Final Answer: 22
```

---

## 🔎 Time & Space Complexity

| Metric           | Complexity |
| ---------------- | ---------- |
| Time Complexity  | O(n)       |
| Space Complexity | O(n)       |

* Each token is visited once.
* Stack stores up to O(n) elements in the worst case.

---

## ✅ What Interviewers Expect

1. Recognize that this is a **stack-based evaluation** problem.
2. Know how to handle **operator precedence implicitly** using postfix.
3. Handle **integer division** with truncation toward zero (a tricky edge case!).
4. Write clean, robust code using maps and functions.

---

## 💬 How to Explain in an Interview

> "I use a stack to simulate the postfix evaluation.
> If I see a number, I push it. If I see an operator, I pop the last two numbers,
> apply the operation, and push the result.
> I use a hashmap to map operators to functions,
> and I handle division carefully to truncate toward zero."

✅ "This approach is O(n) in time and space. Each element is processed exactly once."

---

## 🎯 Related Topics & Practice Problems

* **224. Basic Calculator**
* **227. Basic Calculator II**
* **772. Basic Calculator III**
* **394. Decode String** (stack simulation)

---

## 🧪 Edge Cases to Test

| Input                       | Output | Reason                       |
| --------------------------- | ------ | ---------------------------- |
| `["3","-4","+"]`            | `-1`   | Negative result              |
| `["4","13","5","/","+"]`    | `6`    | Integer division truncation  |
| `["100","200","+","2","/"]` | `150`  | Division after addition      |
| `["-2","3","/"]`            | `0`    | Truncate toward zero, not -1 |

---

**🔥 Mastering this problem prepares you for many parsing and stack-based evaluations in interviews.**





---
---
# 🌡️ Daily Temperatures - Complete Interview Guide

## 📋 Problem Description

**LeetCode 739 - Daily Temperatures (Medium)**

Given an array of integers `temperatures` representing daily temperatures, return an array `answer` where `answer[i]` is the **number of days** you have to wait after the `i-th` day to get a **warmer temperature**.

If there is no future warmer day, keep `answer[i] = 0`.

### 🎯 Examples

| Input | Output | Explanation |
|-------|--------|-------------|
| `[73,74,75,71,69,72,76,73]` | `[1,1,4,2,1,1,0,0]` | Day 0: Next warmer is day 1 (1 day wait) |
| `[30,40,50,60]` | `[1,1,1,0]` | Each day has next day warmer except last |
| `[30,60,90]` | `[1,1,0]` | Strictly increasing except last day |

### 🔍 Detailed Example Breakdown

```
temperatures = [73, 74, 75, 71, 69, 72, 76, 73]
indices      = [ 0,  1,  2,  3,  4,  5,  6,  7]

Day 0 (73°): Next warmer is Day 1 (74°) → wait 1 day
Day 1 (74°): Next warmer is Day 2 (75°) → wait 1 day  
Day 2 (75°): Next warmer is Day 6 (76°) → wait 4 days
Day 3 (71°): Next warmer is Day 5 (72°) → wait 2 days
Day 4 (69°): Next warmer is Day 5 (72°) → wait 1 day
Day 5 (72°): Next warmer is Day 6 (76°) → wait 1 day
Day 6 (76°): No warmer day ahead → 0
Day 7 (73°): No warmer day ahead → 0

Result: [1, 1, 4, 2, 1, 1, 0, 0]
```

---

## 💡 Solution Approach

### 🧠 Key Insights

1. **Monotonic Stack Pattern**: We need to find the "next greater element" for each position
2. **Decreasing Stack**: Maintain a stack of temperatures in decreasing order
3. **Index Tracking**: Store indices to calculate the day differences

### 🎨 Visual Algorithm Walkthrough

```
Input: [73, 74, 75, 71, 69, 72, 76, 73]

Step-by-step execution:

Day 0 (73°): Stack empty → Push (73,0)
Stack: [(73,0)]

Day 1 (74°): 74 > 73 → Pop (73,0), answer[0] = 1-0 = 1 → Push (74,1)  
Stack: [(74,1)]

Day 2 (75°): 75 > 74 → Pop (74,1), answer[1] = 2-1 = 1 → Push (75,2)
Stack: [(75,2)]

Day 3 (71°): 71 < 75 → Just push (71,3)
Stack: [(75,2), (71,3)]

Day 4 (69°): 69 < 71 → Just push (69,4)  
Stack: [(75,2), (71,3), (69,4)]

Day 5 (72°): 72 > 69 → Pop (69,4), answer[4] = 5-4 = 1
             72 > 71 → Pop (71,3), answer[3] = 5-3 = 2
             72 < 75 → Push (72,5)
Stack: [(75,2), (72,5)]

Day 6 (76°): 76 > 72 → Pop (72,5), answer[5] = 6-5 = 1  
             76 > 75 → Pop (75,2), answer[2] = 6-2 = 4
             Push (76,6)
Stack: [(76,6)]

Day 7 (73°): 73 < 76 → Just push (73,7)
Stack: [(76,6), (73,7)]

Final: answer = [1, 1, 4, 2, 1, 1, 0, 0]
```

---

## 💻 Optimized Solution

```python
from typing import List

class Solution:
    def dailyTemperatures(self, daily_temperatures: List[int]) -> List[int]:
        total_days = len(daily_temperatures)
        days_to_wait = [0] * total_days
        
        # Monotonic decreasing stack: stores (temperature, day_index) pairs
        # Stack maintains temperatures in decreasing order from bottom to top
        pending_colder_days_stack = []
        
        for current_day_index, current_temperature in enumerate(daily_temperatures):
            
            # Process all previous colder days that found their warmer day today
            while (pending_colder_days_stack and 
                   pending_colder_days_stack[-1][0] < current_temperature):
                
                previous_colder_temp, previous_colder_day_index = pending_colder_days_stack.pop()
                
                # Calculate days waited from previous colder day to today
                days_waited = current_day_index - previous_colder_day_index
                days_to_wait[previous_colder_day_index] = days_waited
            
            # Add current day to stack (it's waiting for a warmer day)
            pending_colder_days_stack.append((current_temperature, current_day_index))
        
        # Days remaining in stack never found a warmer day (already initialized to 0)
        return days_to_wait
```

### 🔧 Alternative Concise Version

```python
def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
    result = [0] * len(temperatures)
    stack = []  # Stack of indices
    
    for current_index, current_temp in enumerate(temperatures):
        # Pop indices while current temperature is warmer
        while stack and temperatures[stack[-1]] < current_temp:
            previous_index = stack.pop()
            result[previous_index] = current_index - previous_index
        
        stack.append(current_index)
    
    return result
```

---

## 📊 Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Each element pushed and popped at most once |
| **Space** | `O(n)` | Stack can contain up to n elements |

### 🔍 Time Complexity Deep Dive
- **Amortized O(n)**: Though we have nested loops, each element is processed exactly twice:
  1. Once when pushed to stack
  2. Once when popped from stack
- **Total operations**: 2n = O(n)

### 💾 Space Complexity Cases
- **Best Case**: `O(1)` - Strictly increasing temperatures `[30,40,50,60]`
- **Worst Case**: `O(n)` - Strictly decreasing temperatures `[60,50,40,30]`

---

## 🎯 Interview Performance Guide

### 🗣️ **Step 1: Problem Understanding (3-4 minutes)**

**What to communicate to the interviewer:**

> *"Let me break down this problem:*
> - *We need to find, for each day, how many days until a warmer temperature*
> - *This is asking for the 'next greater element' with distance calculation*  
> - *If no warmer day exists, we return 0 for that position*
>
> *Looking at the example [73,74,75,71,69,72,76,73]:*
> - *Day 0 (73°): Day 1 has 74° → 1 day wait*
> - *Day 2 (75°): Day 6 has 76° → 4 days wait*
>
> *This looks like a classic monotonic stack problem where we track pending days waiting for warmer weather."*

### 🧠 **Step 2: Approach Discussion (4-5 minutes)**

**Walk through your strategy:**

> *"My approach uses a monotonic decreasing stack:*
> 
> 1. *I'll maintain a stack of days that haven't found their warmer day yet*
> 2. *For each new day, I'll check if it's warmer than previous days in my stack*
> 3. *If yes, I pop those days and calculate how long they waited*
> 4. *Then I add the current day to the stack*
>
> *The key insight is that we only need to look backwards at days that are still 'waiting' - the stack maintains this efficiently.*
>
> *Time complexity will be O(n) because each day gets pushed and popped at most once."*

### 🔄 **Step 3: Consider Alternative Approaches (2-3 minutes)**

**Mention and dismiss brute force:**

> *"I could solve this with a brute force O(n²) approach - for each day, scan forward until I find a warmer day. But that's inefficient for large inputs.*
>
> *The stack approach is optimal because it avoids redundant comparisons by maintaining which days are still 'pending' their warmer day."*

### 💭 **Step 4: Edge Cases Discussion (2 minutes)**

**Important scenarios to mention:**

> *"Edge cases to consider:*
> - *Single element array → [0]*
> - *Strictly increasing → [1,1,1,0] (each day finds next day warmer)*  
> - *Strictly decreasing → [0,0,0,0] (no day finds warmer)*
> - *All same temperature → [0,0,0,0] (need strictly warmer)*"*

### ⚡ **Step 5: Implementation (10-12 minutes)**

**Live coding best practices:**
- **Start with function signature** and explain your variable naming
- **Use descriptive names**: `pending_colder_days_stack` instead of just `stack`
- **Add comments explaining the algorithm logic**
- **Explain the stack invariant**: "The stack always maintains temperatures in decreasing order"

### 🧪 **Step 6: Testing & Validation (3-4 minutes)**

**Trace through a small example:**

> *"Let me trace [73,74,75] to verify:*
> - *Day 0: Stack empty, push (73,0). Stack: [(73,0)]*
> - *Day 1: 74>73, pop (73,0), answer[0]=1-0=1, push (74,1). Stack: [(74,1)]*  
> - *Day 2: 75>74, pop (74,1), answer[1]=2-1=1, push (75,2). Stack: [(75,2)]*
> - *Result: [1,1,0] ✓*"*

---

## 🚀 Advanced Interview Topics

### 🔄 **Variations You Might Be Asked**

1. **"Find Previous Greater Element"**
   > *"I'd iterate from right to left instead of left to right, maintaining the same stack logic."*

2. **"Find Next Smaller Element"**  
   > *"I'd change the comparison from `<` to `>` in the while loop condition."*

3. **"What if we wanted the actual temperatures, not distances?"**
   > *"I'd store temperatures in the result array instead of calculating index differences."*

### 🎯 **Follow-up Questions**

1. **"Can you optimize space further?"**
   > *"For this problem, O(n) space is optimal since we need to track pending days. However, if temperatures were bounded (like 1-100), we could use counting techniques, but it wouldn't improve worst-case complexity."*

2. **"How would you handle this with streaming data?"**
   > *"The algorithm naturally handles streaming - we process each new temperature as it arrives and can output results for resolved days immediately."*

3. **"What if temperatures could be negative?"**
   > *"The algorithm works identically - we're only comparing relative values, not absolute temperatures."*

---

## ⚠️ Common Interview Pitfalls

| ❌ **Mistake** | ✅ **Better Approach** | 
|----------------|------------------------|
| Using brute force O(n²) | Recognize as monotonic stack pattern |
| Storing temperatures in result | Store day differences, not temperatures |
| Confusing stack order | Maintain decreasing order (larger temps at bottom) |
| Not explaining the invariant | "Stack keeps unresolved days in decreasing temp order" |
| Forgetting edge cases | Mention single element, all same temps, etc. |

---

## 🎖️ What Interviewers Evaluate

### ✅ **Pattern Recognition** 
- **Identifying Monotonic Stack**: Recognizing this as a "next greater element" variant
- **Optimization Awareness**: Understanding why O(n²) brute force is suboptimal

### ✅ **Implementation Skills**
- **Clean Code**: Descriptive variable names and clear logic flow
- **Edge Case Handling**: Considering boundary conditions
- **Space-Time Trade-offs**: Explaining why we use O(n) space for O(n) time

### ✅ **Communication**
- **Algorithm Explanation**: Clearly describing the stack invariant
- **Example Walkthrough**: Demonstrating understanding with concrete examples
- **Complexity Analysis**: Explaining amortized time complexity

---

## 🏆 Pro Interview Tips

### 🎯 **During Problem Discussion:**
1. **Draw the stack states** - Visual representation helps both you and interviewer
2. **Emphasize the invariant** - "Stack maintains decreasing temperatures"
3. **Connect to pattern** - "This is the classic next greater element problem"

### 💬 **During Implementation:**
1. **Narrate your code** - "I'm popping because current temp is warmer"
2. **Use meaningful names** - Shows professional coding habits
3. **Handle edge cases** - Check empty stack before popping

### 🎨 **During Testing:**
1. **Pick simple examples** - `[30,40,50]` is easier to trace than long arrays
2. **Verify the invariant** - Check that stack maintains decreasing order
3. **Test edge cases** - Single element, all same temperatures

### 🚀 **Advanced Discussion:**
- **Mention related problems**: "This pattern appears in largest rectangle in histogram"
- **Discuss variations**: Next smaller, previous greater, circular arrays
- **Real-world applications**: Stock span problem, building view problems

Remember: Interviewers want to see your **problem-solving process** and **communication skills** as much as the correct solution! 🌟
