
# 🔐 20. Valid Parentheses – Complete Interview Guide

**Difficulty:** Easy  
**Tags:** Stack, String  
**Asked by:** Amazon, Facebook, Google, Microsoft  

---

## 📘 Problem Statement

Given a string `s` consisting only of the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine whether the input string is **valid**.

A string is **valid** if:

- Every **open bracket** has a **corresponding closing bracket** of the same type.
- Open brackets are **closed in the correct order**.
- Every **closing bracket** has a **preceding open bracket** of the same type.

---

## 🧪 Examples

### Example 1
```text
Input: s = "()"
Output: true
````

### Example 2

```text
Input: s = "()[]{}"
Output: true
```

### Example 3

```text
Input: s = "(]"
Output: false
```

### Example 4

```text
Input: s = "([])"
Output: true
```

---

## ✅ What Interviewers Expect

During interviews, make sure to:

* 💬 **Explain your choice of data structure** – here, a **stack** helps track unmatched opening brackets.
* 🔁 Simulate character-by-character parsing.
* 🔍 Handle edge cases like `"([)]"` or `"]"`.
* ⏱ Discuss **time/space complexity**.

---

## 🧠 Key Observations

| Symbol      | Action                                       |
| ----------- | -------------------------------------------- |
| `(` `{` `[` | Push to stack (waiting for a closing match)  |
| `)` `}` `]` | Must match the **top** of stack (LIFO check) |

---

## 👨‍💻 Python Code (Clean & Interview-Ready)

```python
from typing import List

class Solution:
    def isValid(self, s: str) -> bool:
        # Mapping of closing brackets to their corresponding opening brackets
        closing_to_opening = {')': '(', '}': '{', ']': '['}
        
        # Stack to keep track of opening brackets
        open_brackets_stack = []

        # Traverse each character in the input string
        for char in s:
            if char in closing_to_opening:
                # If there's no matching opening bracket, return False
                if not open_brackets_stack:
                    return False
                
                # Pop the last opening bracket and check if it matches
                last_open = open_brackets_stack.pop()
                if last_open != closing_to_opening[char]:
                    return False
            else:
                # It's an opening bracket — push to stack
                open_brackets_stack.append(char)
        
        # Stack should be empty if all brackets matched
        return not open_brackets_stack
```

---

## 🔍 Dry Run – Step-by-Step Example

### Input: `s = "{[()]}"`

| Step | Char | Stack            | Action               |
| ---- | ---- | ---------------- | -------------------- |
| 1    | `{`  | \[`{`]           | Push opening bracket |
| 2    | `[`  | \[`{`, `[`]      | Push opening bracket |
| 3    | `(`  | \[`{`, `[`, `(`] | Push opening bracket |
| 4    | `)`  | \[`{`, `[`]      | Match and pop `(`    |
| 5    | `]`  | \[`{`]           | Match and pop `[`    |
| 6    | `}`  | `[]` (empty)     | Match and pop `{`    |

✅ Stack is empty → string is valid

---

## ⏱️ Time & Space Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | O(n)       |
| Space | O(n)       |

* **n** is the length of the input string
* Stack stores unmatched opening brackets (at most n)

---

## 🧪 Edge Cases to Consider

| Input      | Output | Reason                              |
| ---------- | ------ | ----------------------------------- |
| `"]"`      | false  | No opening bracket for closing `]`  |
| `"["`      | false  | No closing bracket for `[`          |
| `"([)]"`   | false  | Incorrect order of closing brackets |
| `"((()))"` | true   | Perfectly nested                    |

---

## 💬 How to Explain During Interviews

* “I’m using a stack to manage open brackets — it helps track the most recent unmatched opening.”
* “When I find a closing bracket, I check if it correctly matches the top of the stack.”
* “If there's ever a mismatch or the stack is empty when a closing bracket appears, the string is invalid.”
* “Finally, if the stack is empty, all brackets were matched correctly.”

---

## 📌 Takeaways

✅ Ideal for stack practice
✅ Covers bracket parsing — common in compilers and interpreters
✅ Easy to whiteboard and simulate live

---

**🚀 Mastering this problem helps with more complex stack simulations like:**

* **Daily Temperatures**
* **Min Stack**
* **Decode String**

---

**🧠 Ready to explain, code, and optimize on the fly!**




---
# 🔗 Valid Parentheses - Complete Interview Guide

## 📋 Problem Description

**LeetCode 20 - Valid Parentheses (Easy)**

Given a string containing only the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is **valid**.

### ✅ Validity Rules
1. **Matching Types**: Open brackets must be closed by the same type of brackets
2. **Correct Order**: Open brackets must be closed in the correct order  
3. **Complete Pairs**: Every close bracket has a corresponding open bracket

### 🎯 Examples

| Input | Output | Explanation |
|-------|--------|-------------|
| `"()"` | `true` | Simple pair matches |
| `"()[]{}"` | `true` | Multiple pairs in sequence |
| `"(]"` | `false` | Wrong closing bracket type |
| `"([)]"` | `false` | Wrong order (nested incorrectly) |
| `"([])"` | `true` | Properly nested brackets |

---

## 💡 Solution Approach

### 🧠 Key Insight
This is a **Last-In-First-Out (LIFO)** problem - perfect for a **Stack**!

- When we see an **opening bracket** → Push to stack
- When we see a **closing bracket** → Pop from stack and verify it matches

### 🎨 Visual Walkthrough

```
Input: "([{}])"

Step 1: '(' → Stack: ['(']
Step 2: '[' → Stack: ['(', '['] 
Step 3: '{' → Stack: ['(', '[', '{']
Step 4: '}' → Pop '{', matches ✓ → Stack: ['(', '[']
Step 5: ']' → Pop '[', matches ✓ → Stack: ['(']
Step 6: ')' → Pop '(', matches ✓ → Stack: []

Result: Stack empty → Valid ✅
```

---

## 💻 Optimized Solution

```python
class Solution:
    def isValid(self, bracket_string: str) -> bool:
        # Mapping of closing brackets to their corresponding opening brackets
        closing_to_opening_map = {
            ')': '(',
            '}': '{', 
            ']': '['
        }
        
        # Stack to track opening brackets
        opening_brackets_stack = []
        
        # Process each character in the string
        for current_bracket in bracket_string:
            # Case 1: Opening bracket - add to stack
            if current_bracket not in closing_to_opening_map:
                opening_brackets_stack.append(current_bracket)
            
            # Case 2: Closing bracket - validate and pop
            else:
                # Check if stack is empty (no matching opening bracket)
                if not opening_brackets_stack:
                    return False
                
                # Pop the most recent opening bracket
                most_recent_opening = opening_brackets_stack.pop()
                
                # Verify it matches the current closing bracket
                expected_opening = closing_to_opening_map[current_bracket]
                if most_recent_opening != expected_opening:
                    return False
        
        # Valid only if all brackets are matched (stack is empty)
        return len(opening_brackets_stack) == 0
```

---

## 📊 Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Single pass through string |
| **Space** | `O(n)` | Stack can hold up to n/2 opening brackets |

### 🔍 Space Complexity Deep Dive
- **Best Case**: `O(1)` - All brackets are immediately closed: `"()()()"`
- **Worst Case**: `O(n)` - All opening brackets: `"((([["`
- **Average Case**: `O(n/2)` - Balanced nesting

---

## 🎯 Interview Performance Guide

### 🗣️ **Step 1: Clarify the Problem (2-3 minutes)**

**What to say to the interviewer:**

> *"Let me make sure I understand the problem correctly:*
> - *We need to validate if brackets are properly matched and nested*  
> - *We have three types: parentheses (), braces {}, and brackets []*
> - *Each opening bracket must have a corresponding closing bracket of the same type*
> - *The order matters - we can't have something like '([)]' because the brackets cross each other*
> 
> *Should I assume the input only contains these bracket characters, or could there be other characters I need to ignore?"*

### 🧠 **Step 2: Discuss Approach (3-4 minutes)**

**Walk through your thinking:**

> *"This problem screams 'stack' to me because:*
> - *We need to remember which opening brackets we've seen*
> - *When we encounter a closing bracket, we need to check against the most recent opening bracket*
> - *This Last-In-First-Out behavior is exactly what a stack provides*
>
> *My approach will be:*
> 1. *Use a stack to track opening brackets*
> 2. *When I see an opening bracket, push it onto the stack*
> 3. *When I see a closing bracket, pop from stack and verify they match*
> 4. *At the end, the stack should be empty for a valid string*"

### 💭 **Step 3: Consider Edge Cases (1-2 minutes)**

**Mention these scenarios:**

> *"Let me think about edge cases:*
> - *Empty string - should return true (vacuously valid)*
> - *Single character - always false*  
> - *All opening brackets - stack won't be empty*
> - *All closing brackets - we'll try to pop from empty stack*
> - *Mismatched types like '(]' - types don't correspond*"

### ⚡ **Step 4: Code Implementation (8-10 minutes)**

**Live coding tips:**
- **Start with the function signature** and explain your parameter names
- **Write comments first**, then fill in the code
- **Use descriptive variable names** - avoid single letters in interviews
- **Explain as you code**: *"I'm using a dictionary here to map closing brackets to opening brackets..."*

### 🧪 **Step 5: Test Your Solution (3-4 minutes)**

**Walk through examples:**

> *"Let me trace through '([{}])':*
> - *'(' → stack: ['(']*
> - *'[' → stack: ['(', '[']*  
> - *'{' → stack: ['(', '[', '{']*
> - *'}' → pop '{', matches ✓ → stack: ['(', '[']*
> - *']' → pop '[', matches ✓ → stack: ['(']*
> - *')' → pop '(', matches ✓ → stack: []*
> - *Stack empty → return true ✅*"

---

## 🚀 Advanced Interview Discussion Points

### 🔄 **Alternative Approaches**

**If the interviewer asks about other solutions:**

1. **Counter Approach** (Less Elegant)
   ```python
   # Count opening vs closing brackets
   # Works only for single bracket type, fails for mixed types
   ```

2. **Recursive Approach** (Overkill)
   ```python
   # Recursively find and remove innermost pairs
   # O(n²) time complexity - not optimal
   ```

### 🎯 **Follow-up Questions You Might Get**

1. **"What if the string contained other characters?"**
   > *"I'd modify the solution to skip non-bracket characters, only processing '()[]{}'."*

2. **"How would you handle a streaming input?"**
   > *"The stack approach works perfectly for streaming - we process characters one by one and can return early if we detect invalidity."*

3. **"Can you optimize space complexity?"**
   > *"For this problem, O(n) space is optimal. We need to remember previous opening brackets. However, if we only had one bracket type, we could use a simple counter."*

---

## ⚠️ Common Interview Mistakes to Avoid

| ❌ **Mistake** | ✅ **Correct Approach** |
|----------------|-------------------------|
| Not handling empty stack | Always check `if not stack` before popping |
| Forgetting final stack check | Return `len(stack) == 0`, not just `True` |
| Using confusing variable names | Use `closing_bracket`, not just `c` |
| Not explaining the algorithm | Walk through your approach step-by-step |
| Rushing to code | Spend time on problem understanding first |

---

## 🎖️ What Interviewers Look For

### ✅ **Technical Skills**
- **Data Structure Choice**: Recognizing this as a stack problem
- **Implementation**: Clean, readable code with proper edge case handling
- **Optimization**: Understanding time/space complexity trade-offs

### ✅ **Communication Skills**  
- **Problem Breakdown**: Explaining the approach before coding
- **Code Narration**: Describing what you're doing as you code
- **Testing Mindset**: Walking through examples and edge cases

### ✅ **Problem-Solving Process**
- **Systematic Approach**: Following a structured problem-solving method
- **Edge Case Awareness**: Thinking beyond the happy path
- **Adaptability**: Handling follow-up questions and modifications

---

## 🏆 Pro Tips for Success

1. **Use the whiteboard/screen effectively** - Draw the stack state as you trace through examples
2. **Think out loud** - Your thought process is as important as the final code
3. **Ask clarifying questions** - Shows attention to detail and communication skills  
4. **Start simple, then optimize** - Get a working solution first
5. **Practice the explanation** - Be ready to explain why stack is the right choice

Remember: The interviewer wants to see how you think, not just whether you know the answer! 🧠✨
