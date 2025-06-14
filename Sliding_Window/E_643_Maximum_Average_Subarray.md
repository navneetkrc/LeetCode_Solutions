# 🎯 LeetCode 643: Maximum Average Subarray I
*Complete Interview Preparation Guide with Runtime Optimizations*

---

## 📋 Problem Statement

**Difficulty**: 🟢 Easy  
**Pattern**: 🪟 Fixed-Size Sliding Window  
**Time Complexity**: ⏰ O(n)  
**Space Complexity**: 💾 O(1)

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a **contiguous subarray** whose **length is equal to `k`** that has the **maximum average value** and return *this value*. Any answer with a calculation error less than `10^-5` will be accepted.

### 📊 Examples

#### Example 1:
```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

#### Example 2:
```
Input: nums = [5], k = 1
Output: 5.00000
```

### 🔒 Constraints
- `n == nums.length`
- `1 <= k <= n <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

---

## 🧠 Interview Discussion Points

### 🔑 Key Insights to Share
> "This is a classic fixed-size sliding window problem. Since we need exactly k elements, we can slide a window of size k across the array and track the maximum sum, then convert to average."

### 💡 Why Fixed-Size Sliding Window?
1. **Fixed constraint**: We need exactly k elements
2. **Optimization**: Instead of recalculating sum for each window (O(n*k)), we slide efficiently (O(n))
3. **Pattern recognition**: "Contiguous subarray of fixed size" → Fixed sliding window

---

## 🗣️ What to Say During Interview

### 1. Problem Understanding (1-2 minutes)
```
"Let me understand the problem:
- We need a contiguous subarray of exactly k elements
- We want the maximum average value
- Since average = sum/k, and k is constant, maximizing average 
  is equivalent to maximizing sum

This is a classic fixed-size sliding window problem."
```

### 2. Approach Explanation (2-3 minutes)
```
"My approach:
1. Calculate sum of first k elements
2. Slide the window: remove leftmost, add rightmost element
3. Track maximum sum encountered
4. Return maximum_sum / k

Time: O(n) - each element visited at most twice
Space: O(1) - only using a few variables"
```

---

## 🎯 Interview Solutions (Progressive Optimization)

### 📚 Level 1: Basic Correct Solution
```python
from typing import List

class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        """
        Basic sliding window approach - clean and correct
        Perfect for demonstrating understanding
        """
        # Calculate initial window sum
        window_sum = sum(nums[:k])
        max_sum = window_sum
        
        # Slide the window
        for i in range(k, len(nums)):
            # Remove leftmost element, add new rightmost element
            window_sum = window_sum - nums[i - k] + nums[i]
            max_sum = max(max_sum, window_sum)
        
        return max_sum / k
```

**🎭 Interview Notes:**
- ✅ Clear and readable
- ✅ Shows pattern understanding
- ✅ Easy to explain and trace through

---

### ⚡ Level 2: Optimized for Performance
```python
from typing import List

class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        """
        Performance-optimized version for follow-up questions
        """
        n = len(nums)  # Cache length to avoid repeated calls
        
        # Edge case optimization - very common in practice
        if k == 1:
            return float(max(nums))
        
        # Calculate initial window sum
        current_sum = sum(nums[:k])
        max_sum = current_sum
        
        # Optimized sliding window
        for i in range(k, n):
            # Single operation: slide window efficiently
            current_sum += nums[i] - nums[i - k]
            # Conditional faster than max() function
            if current_sum > max_sum:
                max_sum = current_sum
        
        return max_sum / k
```

**🎭 Interview Notes:**
- ✅ Shows optimization thinking
- ✅ Handles edge cases
- ✅ Demonstrates performance awareness

---

### 🏆 Level 3: Production-Ready Solution
```python
from typing import List

class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        """
        Production-ready with error handling and optimizations
        """
        n = len(nums)
        
        # Input validation (mention but don't always implement)
        if k > n or k <= 0:
            raise ValueError("Invalid k value")
        
        # Early termination optimizations
        if k == n:
            return sum(nums) / k
        if k == 1:
            return float(max(nums))
        
        # Initialize sliding window
        window_sum = sum(nums[:k])
        max_sum = window_sum
        
        # Slide window and find maximum
        for i in range(k, n):
            window_sum += nums[i] - nums[i - k]
            if window_sum > max_sum:
                max_sum = window_sum
        
        return max_sum / k
```

---

## 🎨 Visual Algorithm Walkthrough

```
Example: nums = [1,12,-5,-6,50,3], k = 4

Initial window: [1,12,-5,-6]
Sum = 1 + 12 + (-5) + (-6) = 2
Max = 2

Step 1: Slide right
Remove nums[0]=1, Add nums[4]=50
[12,-5,-6,50]
Sum = 2 - 1 + 50 = 51
Max = max(2, 51) = 51

Step 2: Slide right  
Remove nums[1]=12, Add nums[5]=3
[-5,-6,50,3]
Sum = 51 - 12 + 3 = 42
Max = max(51, 42) = 51

Final Answer: 51 / 4 = 12.75
```

---

## 📊 Performance Analysis & Interview Discussion

### Time Complexity: O(n)
```
"Each element is accessed at most twice:
- Once when added to window (right expansion)  
- Once when removed from window (left contraction)
- Initial sum() is O(k), but k <= n, so overall O(n)"
```

### Space Complexity: O(1)
```
"We only use a constant amount of extra space:
- window_sum, max_sum, loop variables
- No additional data structures needed"
```

### 🆚 Brute Force Comparison
```python
# Brute Force: O(n * k) - DON'T do this!
def findMaxAverage_bruteforce(self, nums, k):
    max_avg = float('-inf')
    for i in range(len(nums) - k + 1):
        current_sum = sum(nums[i:i+k])  # O(k) operation
        max_avg = max(max_avg, current_sum / k)
    return max_avg
```

---

## 🎯 Interview Performance Tips

### ⭐ What Interviewers Want to See

#### 1. Pattern Recognition (🔥 Critical)
```
"I recognize this as a fixed-size sliding window problem because:
- We need exactly k contiguous elements
- We're looking for an optimal value across all such subarrays
- The constraint is fixed (k doesn't change)"
```

#### 2. Optimization Awareness
```
"After I get the basic solution working, I can optimize by:
- Avoiding repeated len() calls
- Using conditional assignment instead of max()
- Handling edge cases like k=1 specially"
```

#### 3. Edge Case Handling
```
"Important edge cases to consider:
- k = 1: Can directly return max(nums)
- k = n: Return average of entire array
- All negative numbers: Still works correctly
- Single element array: Handles naturally"
```

### 💬 Magic Interview Phrases

- **Pattern Recognition**: "This is a fixed-size sliding window problem"
- **Efficiency**: "Instead of recalculating sums, I'll slide the window efficiently"
- **Complexity**: "Each element is visited at most twice, giving us O(n) time"
- **Optimization**: "I can optimize this further by avoiding function call overhead"

---

## 🔥 Common Interview Follow-ups

### Q1: "What if k can be variable?"
**A1**: "That would become a variable-size sliding window problem, more complex. We'd need different constraints to determine when to expand/shrink the window."

### Q2: "Can you optimize this further?"
**A2**: *Show Level 2 optimizations* "Yes, I can avoid function call overhead and handle edge cases."

### Q3: "What about integer overflow?"
**A3**: "In Python, integers have arbitrary precision. In other languages, I'd consider using long or checking for overflow."

### Q4: "How would you test this?"
**A4**: 
```python
# Test cases to mention:
test_cases = [
    ([1,12,-5,-6,50,3], 4, 12.75),  # Given example
    ([5], 1, 5.0),                  # Single element
    ([-1,-2,-3,-4], 2, -1.5),       # All negative
    ([1,2,3,4,5], 5, 3.0),          # k = n
    ([1,1,1,1], 2, 1.0)             # All same
]
```

---

## ⚡ Optimization Comparison

| Solution | Runtime | Memory | Interview Stage |
|----------|---------|--------|----------------|
| Basic | 100% | Good | ✅ Initial solution |
| Optimized | 85-90% | Good | 🔥 Follow-up question |
| Production | 80-85% | Good | 🏆 Senior role discussion |

---

## 🎭 Interview Timeline (15 minutes)

**Minutes 0-2**: Problem understanding and clarification  
**Minutes 2-5**: Explain approach and draw example  
**Minutes 5-10**: Code basic solution  
**Minutes 10-12**: Test with examples  
**Minutes 12-15**: Discuss optimizations and edge cases  

---

## 🏆 Key Takeaways for Success

### ✅ DO:
- Start with basic correct solution
- Explain your thinking process clearly
- Trace through examples
- Discuss time/space complexity
- Mention optimizations as follow-up

### ❌ DON'T:
- Jump to optimizations immediately
- Sacrifice clarity for micro-optimizations
- Forget to handle edge cases
- Skip complexity analysis
- Over-engineer simple problems

### 🎯 Success Formula:
**Correct Solution + Clear Explanation + Complexity Analysis + Edge Cases = Interview Success!**

Remember: This is an "Easy" problem, so focus on demonstrating **clean code**, **clear communication**, and **systematic thinking** rather than complex algorithms! 🚀
