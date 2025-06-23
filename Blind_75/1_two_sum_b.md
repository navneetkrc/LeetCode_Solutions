# 🧠 Two Sum - Complete Guide with Enhanced Visualizations

## 📋 Problem Statement

Given an array of integers `nums` and an integer `target`, return the **indices** of two numbers that add up to the target.

**Constraints:**
- Each input has exactly one solution
- You cannot use the same element twice
- Return indices in any order

**Example:**
```
Input:  nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Reason: nums[0] + nums[1] = 2 + 7 = 9
```

---

## 🎯 Core Concept: The Complement Strategy

The key insight is thinking in terms of **complements**:

> **If I need two numbers that sum to `target`, and I'm looking at number `x`, then I need to find `target - x`**

```
x + complement = target
complement = target - x
```

**Visual Example:**
```
Target = 9, Current number = 2
    ↓
Complement = 9 - 2 = 7
    ↓
"Have I seen 7 before?"
```

This transforms the problem from "find two numbers that sum to target" into "for each number, check if its complement exists."

---

## 🔄 Algorithm Evolution

### ❌ Naive Approach (Brute Force)
```python
# O(n²) - Check every pair
for i in range(len(nums)):
    for j in range(i+1, len(nums)):
        if nums[i] + nums[j] == target:
            return [i, j]
```

**Problem:** We repeatedly search for complements in the remaining array.

### ✅ Optimized Approach (HashMap)
```python
# O(n) - Remember what we've seen
seen = {}
for i, num in enumerate(nums):
    complement = target - num
    if complement in seen:
        return [seen[complement], i]
    seen[num] = i
```

**Key Insight:** Use a HashMap to remember previous numbers and their indices for instant lookup.

---

## 🎬 Step-by-Step Visualization

Let's trace through: `nums = [2, 6, 11, 15, 7]`, `target = 9`

### The HashMap as Memory
Think of `seen` as your **memory notebook** where you write down:
- **Key:** "What number did I see?"
- **Value:** "At which index?"

```
seen = { number: index }
```

### Execution Trace

```
Array: [2, 6, 11, 15, 7]
Index:  0  1   2   3  4
Target: 9
```

**Step 1:** `i=0, num=2`
```
complement = 9 - 2 = 7
seen = {} (empty)
Is 7 in seen? NO
Action: Remember "I saw 2 at index 0"
seen = {2: 0}
```

**Step 2:** `i=1, num=6`
```
complement = 9 - 6 = 3
seen = {2: 0}
Is 3 in seen? NO
Action: Remember "I saw 6 at index 1"
seen = {2: 0, 6: 1}
```

**Step 3:** `i=2, num=11`
```
complement = 9 - 11 = -2
seen = {2: 0, 6: 1}
Is -2 in seen? NO
Action: Remember "I saw 11 at index 2"
seen = {2: 0, 6: 1, 11: 2}
```

**Step 4:** `i=3, num=15`
```
complement = 9 - 15 = -6
seen = {2: 0, 6: 1, 11: 2}
Is -6 in seen? NO
Action: Remember "I saw 15 at index 3"
seen = {2: 0, 6: 1, 11: 2, 15: 3}
```

**Step 5:** `i=4, num=7` ⭐
```
complement = 9 - 7 = 2
seen = {2: 0, 6: 1, 11: 2, 15: 3}
Is 2 in seen? YES! Found at index 0
Action: Return [0, 4]
```

### Visual Summary Table

| Step | Current | Complement | Memory Check | Action | Memory State |
|------|---------|------------|--------------|--------|--------------|
| 1 | `2` at `0` | `7` | 7 not found | Store 2→0 | `{2:0}` |
| 2 | `6` at `1` | `3` | 3 not found | Store 6→1 | `{2:0, 6:1}` |
| 3 | `11` at `2` | `-2` | -2 not found | Store 11→2 | `{2:0, 6:1, 11:2}` |
| 4 | `15` at `3` | `-6` | -6 not found | Store 15→3 | `{2:0, 6:1, 11:2, 15:3}` |
| 5 | `7` at `4` | `2` | **2 found at 0!** | **Return [0,4]** | ✅ **DONE** |

---

## 💡 The "Aha!" Moment

The beauty of this algorithm lies in the **asymmetric search**:

```
When we're at index 4 looking at number 7:
- We need complement 2
- We don't search forward for 2 (expensive)
- We check our memory: "Did I see 2 before?" (instant)
- Yes! It was at index 0
- Return [0, 4]
```

**Key Insight:** We only need to look backward, not forward, because we'll eventually encounter both numbers in a valid pair.

---

## 🔧 Complete Implementation

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # HashMap to store {number: index}
        seen = {}
        
        # Enumerate gives us both index and value
        for i, num in enumerate(nums):
            # Calculate what number we need to find
            complement = target - num
            
            # Check if we've seen the complement before
            if complement in seen:
                # Found it! Return both indices
                return [seen[complement], i]
            
            # Haven't found complement yet, remember this number
            seen[num] = i
        
        # Problem guarantees a solution exists
        return []
```

### Understanding `enumerate()`

```python
nums = [2, 7, 11, 15]
for i, num in enumerate(nums):
    print(f"Index: {i}, Value: {num}")

# Output:
# Index: 0, Value: 2
# Index: 1, Value: 7
# Index: 2, Value: 11
# Index: 3, Value: 15
```

---

## 📊 Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Single pass through array |
| **Space** | `O(n)` | HashMap stores up to n elements |

### Why O(n) Time?
- We visit each element exactly once
- HashMap lookup is O(1) average case
- Total: n × O(1) = O(n)

### Why O(n) Space?
- In worst case, we store all elements in HashMap
- This happens when the solution is the last two elements

---

## 🎯 Interview Mastery Tips

### What Interviewers Want to See

1. **Problem Understanding**
   - "I need to find two indices, not values"
   - "I can't use the same element twice"
   - "There's exactly one solution"

2. **Algorithmic Thinking**
   - Start with brute force, then optimize
   - Explain the complement insight
   - Show how HashMap eliminates nested loops

3. **Implementation Skills**
   - Clean, readable code
   - Proper variable naming
   - Handle edge cases (though problem guarantees solution)

### Common Pitfalls to Avoid

❌ **Returning values instead of indices**
```python
return [num, complement]  # Wrong!
return [seen[complement], i]  # Correct!
```

❌ **Using the same element twice**
```python
# This is prevented by checking seen BEFORE storing current number
if complement in seen:  # Check first
    return [seen[complement], i]
seen[num] = i  # Store after
```

❌ **Overcomplicating the solution**
```python
# Don't need to check if current index equals stored index
# Our algorithm naturally prevents this
```

### Follow-up Questions You Might Get

1. **"What if no solution exists?"**
   - Problem guarantees one exists, but we could return `None` or `[-1, -1]`

2. **"What if multiple solutions exist?"**
   - Return any valid pair (our algorithm returns the first found)

3. **"Can you solve it without extra space?"**
   - Yes, but it requires O(n log n) time with sorting + two pointers

4. **"What about duplicate numbers?"**
   - Our algorithm handles duplicates correctly

---

## 🧠 Mental Models for Understanding

### Model 1: The Detective Analogy
You're a detective looking for two criminals whose ages sum to 50:
- You interview people one by one
- For each person (age X), you ask: "Have I seen someone aged (50-X)?"
- You keep a notebook of everyone you've met
- When you find a match, you've solved the case!

### Model 2: The Puzzle Piece Analogy
You have puzzle pieces that need to fit together to make a target sum:
- Each piece has a specific "complement" that fits with it
- You examine pieces one by one
- You remember each piece you've seen
- When you find a piece whose complement you've already seen, you're done!

### Model 3: The Shopping Analogy
You have a budget of $9 and need to buy exactly 2 items:
- You walk through the store, checking prices
- For each item ($X), you think: "I need something that costs $(9-X)"
- You remember all prices you've seen
- When you find an item whose complement price you've seen, buy both!

---

## 🎁 Related Problems to Practice

Once you master Two Sum, try these variations:

1. **Three Sum** - Find three numbers that sum to target
2. **Two Sum II** - Input array is sorted
3. **Two Sum III** - Design a data structure
4. **Four Sum** - Find four numbers that sum to target
5. **Two Sum BST** - Find two nodes in BST that sum to target

Each builds on the core complement concept you've learned here!

---

## 🏆 Summary

The Two Sum problem teaches us:

1. **Transform the problem:** Instead of "find two numbers," think "find one number's complement"
2. **Trade space for time:** Use extra memory to avoid repeated work
3. **HashMap is powerful:** Constant-time lookups enable linear solutions
4. **One-pass algorithms:** Sometimes you can solve problems without multiple iterations

Master this pattern, and you'll recognize it in many other problems!
