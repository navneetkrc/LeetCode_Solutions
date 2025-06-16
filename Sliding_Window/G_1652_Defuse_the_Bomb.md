# 🎯 Defuse the Bomb - LeetCode 1652 Interview Guide

## 📋 Problem Description

You have a bomb to defuse, and your time is running out! Your informer will provide you with a **circular array** `code` of length `n` and a key `k`.

To decrypt the code, you must replace every number. **All numbers are replaced simultaneously.**

### Rules:
- **If k > 0**: Replace the i-th number with the sum of the **next k numbers**
- **If k < 0**: Replace the i-th number with the sum of the **previous k numbers**  
- **If k = 0**: Replace the i-th number with **0**

> **🔄 Circular Array**: The next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`

### Examples:

| Input | k | Output | Explanation |
|-------|---|--------|-------------|
| `[5,7,1,4]` | `3` | `[12,10,16,13]` | Each number = sum of next 3: `[7+1+4, 1+4+5, 4+5+7, 5+7+1]` |
| `[1,2,3,4]` | `0` | `[0,0,0,0]` | When k=0, all numbers become 0 |
| `[2,4,9,3]` | `-2` | `[12,5,6,13]` | Each number = sum of previous 2: `[3+9, 2+3, 4+2, 9+4]` |

---

## 💡 Solution Approach: Sliding Window

The key insight is to use a **sliding window** of size `|k|` and efficiently move it around the circular array.

### 🧠 Core Strategy:
1. **Initialize**: Create a sliding window of the correct size
2. **Position**: Place the window at the right starting position based on k's sign
3. **Slide**: Move the window one position at a time, updating the sum efficiently
4. **Map**: For each window position, determine which result index it corresponds to

```python
from typing import List

class Solution:
    def decrypt(self, code: List[int], k: int) -> List[int]:
        array_length = len(code)
        decrypted_result = [0] * array_length
        
        # Early return for k=0 case - all elements become 0
        if k == 0:
            return decrypted_result
        
        # Initialize sliding window pointers
        window_left = 0
        window_sum = 0
        window_size = abs(k)
        
        # Slide window across array_length + window_size positions
        # This ensures we cover all necessary positions for the circular array
        for window_right in range(array_length + window_size):
            # Expand window: add current element to sum
            window_sum += code[window_right % array_length]
            
            # Contract window: remove leftmost element if window exceeds target size
            if window_right - window_left + 1 > window_size:
                window_sum -= code[window_left % array_length]
                window_left = (window_left + 1) % array_length
            
            # When window reaches target size, record the sum for appropriate index
            if window_right - window_left + 1 == window_size:
                if k > 0:
                    # For positive k: current window sums the "next k" elements for the element just before window_left
                    result_index = (window_left - 1) % array_length
                    decrypted_result[result_index] = window_sum
                elif k < 0:
                    # For negative k: current window sums the "previous k" elements for the element just after window_right
                    result_index = (window_right + 1) % array_length
                    decrypted_result[result_index] = window_sum
        
        return decrypted_result
```

---

## 🎯 Interview Expectations & Key Points to Discuss

### 1. **Problem Understanding (2-3 minutes)**
**What you should verbalize:**
- "This is a circular array problem where I need to replace each element with a sum"
- "The key insight is that all replacements happen simultaneously, so I can't modify the original array during calculation"
- "I need to handle three cases: k>0 (next elements), k<0 (previous elements), k=0 (zeros)"

**🔍 Clarifying Questions to Ask:**
- "Should I modify the input array in-place or return a new array?"
- "Can k be larger than the array length?"
- "Are there any edge cases with very small arrays I should consider?"

### 2. **Algorithm Design (3-4 minutes)**
**Key observations to share:**
- "I'll use a sliding window approach since we're computing sums of consecutive elements"
- "The tricky part is mapping the window position to the correct result index due to the circular nature"
- "For k>0, when my window covers positions [i, i+k-1], this sum goes to result[i-1]"
- "For k<0, when my window covers positions [i-k+1, i], this sum goes to result[i+1]"

### 3. **Implementation Strategy (1-2 minutes)**
**Technical decisions to explain:**
- "I'll iterate for n + |k| positions to ensure I cover all necessary window positions"
- "Using modular arithmetic (%) to handle the circular array wrapping"
- "Maintaining window sum efficiently by adding new elements and removing old ones"

### 4. **Complexity Analysis**
**Time Complexity:** O(n + |k|) = O(n) since |k| ≤ n-1
**Space Complexity:** O(1) extra space (not counting the output array)

**What to say:** "The sliding window approach gives us linear time complexity because each element is added and removed from the window exactly once."

### 5. **Edge Cases to Mention**
- k = 0 (trivial case)
- k = n-1 or k = -(n-1) (maximum window size)
- Array of length 1
- k positive vs negative boundary conditions

---

## 🚀 Interview Tips

### ✅ **DO:**
- Start with a brute force solution if stuck, then optimize
- Draw out the circular array and window positions for k=3 and k=-2 examples
- Explain the modular arithmetic clearly: "When I use `(i + k) % n`, I'm wrapping around the circular array"
- Test your logic with the given examples step by step

### ❌ **DON'T:**
- Jump straight to the sliding window without explaining why it works
- Forget to handle the circular array wrapping
- Mix up the result index mapping for positive vs negative k
- Assume the interviewer understands modular arithmetic without explanation

### 🎪 **Bonus Points:**
- Mention that this problem could also be solved with a straightforward O(n×|k|) approach, but sliding window is more efficient
- Discuss how the problem relates to other circular array problems
- Show awareness of integer overflow concerns for very large sums (though not relevant given the constraints)

---

## 🧪 Test Cases to Walk Through

```python
# Test Case 1: k > 0
code = [5, 7, 1, 4], k = 3
# Window positions: [7,1,4] → result[0]=12, [1,4,5] → result[1]=10, etc.

# Test Case 2: k < 0  
code = [2, 4, 9, 3], k = -2
# Window positions: [9,3] → result[0]=12, [2,3] → result[1]=5, etc.

# Test Case 3: Edge case
code = [1], k = 1
# Only one element, next element wraps to itself: result = [1]
```

Remember: **The goal is not just to solve the problem, but to demonstrate clear thinking, good communication, and systematic problem-solving approach!**
