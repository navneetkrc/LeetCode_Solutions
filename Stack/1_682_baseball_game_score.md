# 🏈 LeetCode 682: Baseball Game - Complete Interview Guide

## 📋 Problem Description

You are keeping scores for a baseball game with special rules. Starting with an empty record, you process a series of operations and return the final sum of all valid scores.

### 🎯 Operations Rules
| Operation | Symbol | Action |
|-----------|--------|--------|
| **Integer** | `"5"`, `"-2"` | Record the integer as a new score |
| **Add Previous Two** | `"+"` | Record sum of last two scores |
| **Double Last** | `"D"` | Record double of the last score |
| **Cancel Last** | `"C"` | Remove the last score from record |

### 🔍 Examples Walkthrough

#### Example 1: `["5","2","C","D","+"]` → Output: `30`
```
Step by step:
"5"  → [5]           (add 5)
"2"  → [5, 2]        (add 2)  
"C"  → [5]           (cancel last: remove 2)
"D"  → [5, 10]       (double last: 5×2=10)
"+"  → [5, 10, 15]   (sum last two: 5+10=15)

Final sum: 5 + 10 + 15 = 30
```

#### Example 2: `["5","-2","4","C","D","9","+","+"]` → Output: `27`
```
"5"   → [5]                    (add 5)
"-2"  → [5, -2]               (add -2)
"4"   → [5, -2, 4]            (add 4)
"C"   → [5, -2]               (cancel 4)
"D"   → [5, -2, -4]           (double -2: -2×2=-4)
"9"   → [5, -2, -4, 9]        (add 9)
"+"   → [5, -2, -4, 9, 5]     (sum last two: -4+9=5)
"+"   → [5, -2, -4, 9, 5, 14] (sum last two: 9+5=14)

Final sum: 5 + (-2) + (-4) + 9 + 5 + 14 = 27
```

## 💻 Solution Code

```python
---
from typing import List

class Solution:
    def calPoints(self, operations: List[str]) -> int:
        score_record = []

        for operation in operations:
            if operation == '+':
                # Add the sum of the last two scores
                last_score = score_record[-1]
                second_last_score = score_record[-2]
                score_record.append(last_score + second_last_score)

            elif operation == 'D':
                # Double the last score
                last_score = score_record[-1]
                score_record.append(2 * last_score)

            elif operation == 'C':
                # Remove the last valid score
                score_record.pop()

            else:
                # Add a new integer score
                score_record.append(int(operation))
        
        # Return the total score after all operations
        return sum(score_record)

```
---

## 💻 Solution Code

```python
from typing import List

class Solution:
    def calPoints(self, operations: List[str]) -> int:
        """
        Calculate baseball game score with special operations.
        
        Args:
            operations: List of string operations to process
            
        Returns:
            Sum of all valid scores after processing operations
        """
        valid_scores_record = []  # Stack to maintain current valid scores
        
        for current_operation in operations:
            if current_operation == '+':
                # Add sum of last two scores
                sum_of_last_two = (valid_scores_record[-1] + 
                                 valid_scores_record[-2])
                valid_scores_record.append(sum_of_last_two)
            
            elif current_operation == 'D':
                # Double the last score
                doubled_last_score = valid_scores_record[-1] * 2
                valid_scores_record.append(doubled_last_score)
            
            elif current_operation == 'C':
                # Cancel (remove) the last score
                valid_scores_record.pop()
            
            else:
                # It's an integer score - convert and add
                integer_score = int(current_operation)
                valid_scores_record.append(integer_score)
        
        # Return sum of all remaining valid scores
        total_final_score = sum(valid_scores_record)
        return total_final_score
```

---

## 🧪 Examples

### Example 1
```text
Input:  ops = ["5","2","C","D","+"]
Output: 30
Explanation:
"5" → [5]  
"2" → [5, 2]  
"C" → [5]  
"D" → [5, 10]  
"+" → [5, 10, 15]  
Sum = 5 + 10 + 15 = 30
````

### Example 2

```text
Input:  ops = ["5","-2","4","C","D","9","+","+"]
Output: 27
```

---

## ✅ Interviewer Expectations

During interviews, communicate:

* **Choice of data structure:** Stack (LIFO) is ideal for accessing recent scores.
* **How each operation is handled.**
* **Space/Time Complexity.**
* **Edge case handling.**

Also, *speak aloud while coding* — e.g., "For '+', I need the last two scores from the stack".

---

## 🧠 Key Observations

| Operation | Action                              |
| --------- | ----------------------------------- |
| `"x"`     | Push integer x onto the score stack |
| `"+"`     | Push sum of last two scores         |
| `"D"`     | Push 2 × last score                 |
| `"C"`     | Remove last score                   |

---

## 🔍 Step-by-Step Dry Run

### Input:

```python
["5", "2", "C", "D", "+"]
```

| Step | Operation | Stack State    | Explanation              |
| ---- | --------- | -------------- | ------------------------ |
| 1    | "5"       | \[5]           | Add 5                    |
| 2    | "2"       | \[5, 2]        | Add 2                    |
| 3    | "C"       | \[5]           | Remove last (2)          |
| 4    | "D"       | \[5, 10]       | Double last (5 → 10)     |
| 5    | "+"       | \[5, 10, 15]   | Sum last two (5+10 → 15) |
|      |           | **Total = 30** |                          |

---

## ⏱️ Time & Space Complexity

* **Time:** O(n), where n = number of operations
* **Space:** O(n), for storing valid scores

---

## 💬 What to Say During Interviews

* ✅ “I’m using a stack to maintain scores dynamically.”
* ✅ “I’ll iterate through each operation and handle it based on type.”
* ✅ “For '+', I’ll use the top two scores; for 'C', I’ll remove the last.”
* ✅ “This is a simulation problem, and a stack gives O(1) access to recent values.”

---

## 📌 Edge Cases to Discuss

* Only one element followed by "C"
* Long sequence of operations, check 32-bit range
* Multiple "+" operations (need at least two scores before "+")
* "D" when only one score present (valid as per constraints)

---

## 🧠 Tip for Whiteboarding

Use symbols:

* ➕ for '+'
* ✖️2 for 'D'
* ❌ for 'C'
* 📥 for pushing integers

And label stack state at each step.

---

## 🎯 Key Thought

This problem tests your **ability to simulate** rules using the **right data structure** and cleanly organize logic. It’s an excellent warm-up for stack-based or simulation interviews.

---
## 🎯 Interview Strategy & Expectations

### 🗣️ Step-by-Step Interview Approach

#### 1. **Problem Understanding** (2-3 minutes)
**What to say:**
> "Let me understand this problem. We're processing baseball operations sequentially, maintaining a record of valid scores. The key operations are: adding integers, summing last two scores (+), doubling last score (D), and canceling last score (C). Finally, we return the sum of all remaining scores."

**Ask clarifying questions:**
- "Are the operations guaranteed to be valid?" ✅ (Yes, per constraints)
- "Can we have negative scores?" ✅ (Yes, from examples)
- "What's the expected time/space complexity?" 

#### 2. **Approach Explanation** (3-4 minutes)
**What to say:**
> "This is a classic **stack problem**. We need to:
> - Process operations sequentially
> - Maintain access to recent scores for '+' and 'D' operations  
> - Remove scores for 'C' operation
> - A stack (list) is perfect since we only need access to the most recent elements"

#### 3. **Algorithm Walkthrough** (5-7 minutes)
**Walk through Example 1 step by step:**
```
operations = ["5","2","C","D","+"]
stack = []

"5": stack = [5]
"2": stack = [5, 2] 
"C": stack = [5]        (pop last)
"D": stack = [5, 10]    (5 * 2 = 10)
"+": stack = [5, 10, 15] (5 + 10 = 15)

Sum = 30
```

#### 4. **Code Implementation** (8-10 minutes)
**Start with basic structure, then add details:**
```python
# Step 1: Basic structure
def calPoints(self, operations):
    stack = []
    for op in operations:
        # Process each operation
    return sum(stack)

# Step 2: Add operation handling
# Step 3: Add detailed variable names and comments
```

#### 5. **Testing & Edge Cases** (3-5 minutes)
**Mention these test cases:**
- Empty operations (not possible per constraints)
- Single operation: `["5"]` → `5`
- All cancels: `["1","C"]` → `0`
- Negative numbers: `["-2","D"]` → `[-2, -4]` → `-6`

---

## 🔍 Key Interview Observations to Share

### 💡 **Algorithm Insights**
1. **"This is a stack-based simulation problem"** - Immediately identify the pattern
2. **"We only need the most recent 1-2 elements"** - Justify stack choice
3. **"Operations are processed sequentially"** - Emphasize order matters

### ⚡ **Complexity Analysis**
```
Time Complexity: O(n) - single pass through operations
Space Complexity: O(n) - worst case: all operations add scores
```

### 🚨 **Edge Cases to Mention**
- **Negative integers**: `"-2"` should be handled as integer conversion
- **Multiple consecutive operations**: `"+","+"` both reference different "last two"
- **Order dependency**: Operations must be processed sequentially

### 🎯 **Code Quality Points**
- **Descriptive variable names**: `valid_scores_record` vs `stack`
- **Clear operation separation**: Each operation in distinct if/elif block
- **Inline comments**: Explain non-obvious operations like `stack[-1] + stack[-2]`

---

## 🌟 Interview Success Tips

### ✅ **Do This:**
- **Think aloud**: "I'm using a stack because..."
- **Test with examples**: Walk through provided examples
- **Consider edge cases**: "What if we have consecutive '+' operations?"
- **Optimize variable names**: Make code self-documenting
- **Explain time/space complexity**: Show you understand efficiency

### ❌ **Avoid This:**
- Jumping straight to code without explanation
- Using unclear variable names like `s`, `arr`, `res`
- Forgetting to handle integer conversion: `int(operation)`
- Not testing the solution with given examples
- Overcomplicating with unnecessary data structures

### 🎯 **Bonus Points:**
- **Alternative approaches**: "We could use a list, but stack operations are more intuitive"
- **Real-world applications**: "Similar to undo/redo functionality or expression evaluation"
- **Scalability considerations**: "For larger inputs, we might consider..."

---

## 📊 Complexity Summary

| Aspect | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Single pass through operations |
| **Space** | `O(n)` | Stack can grow up to n elements |
| **Operations** | `O(1)` | Each operation is constant time |

This problem tests your ability to recognize stack patterns, implement clean simulation logic, and handle multiple operation types efficiently. Focus on clear communication and systematic problem-solving approach!
