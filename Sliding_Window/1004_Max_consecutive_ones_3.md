# 🧠 Leetcode 1004 - Max Consecutive Ones III

## 📘 Problem Statement

Given a binary array `nums` and an integer `k`, return the **maximum number of consecutive 1's** in the array if you can flip **at most `k` 0's**.

### 🔍 Examples

**Example 1:**

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2  
Output: 6  
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
```

**Example 2:**

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3  
Output: 10
```

### 📜 Constraints

* `1 <= nums.length <= 10^5`
* `nums[i]` is either `0` or `1`
* `0 <= k <= nums.length`

---

## 💡 Key Observations

* You are allowed to flip **at most `k` zeroes**.
* You need to find the **longest window** (subarray) such that it contains **at most `k` 0's**.
* This can be solved using the **Sliding Window (Two Pointers)** technique.

---

## 🧠 Intuition & Strategy

To solve this problem, we use the **Sliding Window (Two Pointers)** technique:

✅ Treat the problem as finding the longest subarray that contains **at most `k` zeroes**.

✅ As we iterate over the array:

* Expand the window by moving the `right` pointer.
* Track how many zeroes are in the current window.
* If zero count exceeds `k`, shrink the window from the `left` until it’s valid again.
* Keep track of the maximum window size throughout.

This ensures we only traverse the array **once**, making it very efficient.

---

## ✅ Clean Python Code (Interview-Ready)

```python
def longestOnes(nums: list[int], k: int) -> int:
    left = 0        # Start of the window
    zero_count = 0  # Number of 0s in the current window
    max_len = 0     # Maximum length of valid window

    for right in range(len(nums)):
        if nums[right] == 0:
            zero_count += 1

        # Shrink the window from the left if we exceed k flips
        while zero_count > k:
            if nums[left] == 0:
                zero_count -= 1
            left += 1

        # Update the maximum length
        max_len = max(max_len, right - left + 1)

    return max_len
```
---

## ⏱️ Time & Space Complexity

| Metric           | Value  |
| ---------------- | ------ |
| Time Complexity  | `O(n)` |
| Space Complexity | `O(1)` |

---

## 🧑‍💼 What Interviewers Expect You to Explain

0. **Problem Understanding**

    * You can flip at most `k` 0's.
    * Find the **longest subarray** that contains **only 1's after flipping**.

1. **Sliding Window Strategy:**

   * Maintain a window `[left, right]` that is valid (i.e., has at most `k` zeroes).
   * Expand the window to the right.
   * Shrink it from the left when the window becomes invalid.

2. **Why Greedy Sliding Window Works:**

   * We're always keeping the window as large as possible while staying within the allowed flip count.
   * No need to check all subarrays — we do it in one pass.

3. **Time and Space Complexity:**

   * `O(n)` time since each element is visited at most twice.
   * `O(1)` space (no extra data structures needed).

4. **Edge Cases to Consider:**

   * `nums` has all 1s or all 0s.
   * `k = 0` (no flips allowed).
   * `k >= number of 0s` (can flip all 0s).

---

## 🧠 Interview Tips

* ✅ Talk through your thought process before jumping into code.
* ✅ Mention brute force briefly and explain why it’s inefficient (`O(n^2)`).
* ✅ Introduce the sliding window technique and why it’s optimal here.
* ✅ Explain the key idea: track the number of `0`s in a window and adjust when it exceeds `k`.
* ✅ Write clean, readable code with clear variable names and concise logic.
* ✅ Justify your solution’s correctness and efficiency.

---

## 🧪 Sample Test Cases

```python
print(longestOnes([1,1,1,0,0,0,1,1,1,1,0], 2))  # Output: 6
print(longestOnes([0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], 3))  # Output: 10
print(longestOnes([1,1,1,1], 0))  # Output: 4
print(longestOnes([0,0,0], 0))    # Output: 0
print(longestOnes([0,0,0], 3))    # Output: 3
```

---

## ✅ Summary

* Sliding window is perfect when you’re dealing with subarrays and constraints like "at most k something".
* Maintain window validity by tracking zero flips.
* Interviewers care about **clarity**, **correctness**, and **communication** of trade-offs and design choices.


---

## 🧭 Visual Intuition

Imagine the window as a horizontal bar sliding across the array:

```
nums:        [1, 1, 0, 0, 1, 1, 1]
              ↑           ↑
            left        right

Flips used:  2 (positions of 0s)
```

As long as the number of 0's in the window ≤ `k`, we keep expanding. If it exceeds, shrink from the left.

---

## 📎 Related Topics

* Sliding Window
* Greedy
* Two Pointers
* Arrays

---

# 🎯 LeetCode 1004: Max Consecutive Ones III

## 📋 Problem Overview

**Given:** A binary array `nums` and integer `k`  
**Find:** Maximum number of consecutive 1's after flipping at most `k` zeros

---

## 🔍 Problem Examples

### Example 1
```
Input:  nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6

Visual:
Original: [1,1,1,0,0,0,1,1,1,1,0]
          └─────┘ └─────────┘
Flipped:  [1,1,1,0,0,1,1,1,1,1,1]
                    └─────────┘ (length = 6)
```

### Example 2
```
Input:  nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10

We can flip 3 zeros to get a sequence of 10 consecutive ones
```

---

## 💡 Key Insight: Sliding Window Pattern

This problem is asking: **"Find the longest subarray with at most k zeros"**

### Why Sliding Window?
- We need to find a **contiguous subarray** (window)
- We have a **constraint** (at most k zeros)
- We want to **maximize** the window size

---

## 🎨 Visual Algorithm Walkthrough

Let's trace through Example 1: `nums = [1,1,1,0,0,0,1,1,1,1,0]`, `k = 2`

```
Step-by-step sliding window:

Initial: left=0, right=0, zeros=0, max_len=0
[1,1,1,0,0,0,1,1,1,1,0]
 ↑
L,R

Step 1: right=0, nums[0]=1, zeros=0 ✓
Window: [1] → max_len = 1

Step 2: right=1, nums[1]=1, zeros=0 ✓
Window: [1,1] → max_len = 2

Step 3: right=2, nums[2]=1, zeros=0 ✓
Window: [1,1,1] → max_len = 3

Step 4: right=3, nums[3]=0, zeros=1 ✓ (first zero)
Window: [1,1,1,0] → max_len = 4

Step 5: right=4, nums[4]=0, zeros=2 ✓ (second zero)
Window: [1,1,1,0,0] → max_len = 5

Step 6: right=5, nums[5]=0, zeros=3 ✗ (exceeds k=2)
Need to shrink window from left:
- Move left from 0→1→2→3 until we remove a zero
- At left=3, we remove the first zero: zeros=2 ✓
Window: [0,0,0] → current_len = 3

Continue expanding...
Eventually find optimal window of length 6
```

---

## 🔧 Algorithm Template

### Core Strategy
```python
def sliding_window_pattern(nums, k):
    left = 0
    constraint_count = 0  # Track constraint violations
    max_result = 0
    
    for right in range(len(nums)):
        # 1. Expand window - update constraint
        if violates_constraint(nums[right]):
            constraint_count += 1
            
        # 2. Contract window - restore constraint
        while constraint_count > k:
            if violates_constraint(nums[left]):
                constraint_count -= 1
            left += 1
            
        # 3. Update result
        max_result = max(max_result, right - left + 1)
    
    return max_result
```

---

## ✅ Complete Solution

```python
def longestOnes(nums: list[int], k: int) -> int:
    """
    Find maximum consecutive 1's after flipping at most k zeros.
    
    Approach: Sliding Window
    - Maintain window with at most k zeros
    - Expand right, contract left when needed
    
    Time: O(n) - each element visited at most twice
    Space: O(1) - only using pointers and counters
    """
    left = 0        # Left boundary of sliding window
    zero_count = 0  # Number of zeros in current window
    max_length = 0  # Maximum valid window size found
    
    for right in range(len(nums)):
        # Expand window: include nums[right]
        if nums[right] == 0:
            zero_count += 1
        
        # Contract window: ensure at most k zeros
        while zero_count > k:
            if nums[left] == 0:
                zero_count -= 1
            left += 1
        
        # Update maximum window size
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

---

## 🧪 Test Cases & Verification

```python
def test_longestOnes():
    # Basic examples
    assert longestOnes([1,1,1,0,0,0,1,1,1,1,0], 2) == 6
    assert longestOnes([0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], 3) == 10
    
    # Edge cases
    assert longestOnes([1,1,1,1], 0) == 4        # All ones, no flips needed
    assert longestOnes([0,0,0,0], 0) == 0        # All zeros, no flips allowed
    assert longestOnes([0,0,0], 3) == 3          # Can flip all zeros
    assert longestOnes([1], 1) == 1              # Single element
    assert longestOnes([0,1,0,1], 1) == 2        # Alternating pattern
    
    print("✅ All tests passed!")

test_longestOnes()
```

---

## 📊 Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Each element visited at most twice (once by right, once by left) |
| **Space** | `O(1)` | Only using integer variables for pointers and counters |

### Why O(n) and not O(n²)?
- Left pointer only moves forward, never backwards
- Total movements of left pointer ≤ n
- Each element processed at most twice

---

## 🎯 Interview Success Tips

### 🗣️ **Communication Strategy**
1. **Clarify the problem**: "So we can flip at most k zeros to maximize consecutive ones?"
2. **Identify pattern**: "This looks like a sliding window problem with a constraint"
3. **Explain approach**: "I'll maintain a window with at most k zeros"
4. **Code step-by-step**: Walk through the algorithm logic
5. **Test with examples**: Verify with given examples and edge cases

### ⚡ **Common Follow-ups**
- **"What if we want exactly k flips?"** → Modify condition to `zero_count != k`
- **"What if we can flip 1s to 0s instead?"** → Track ones instead of zeros
- **"Return the actual subarray?"** → Store left and right indices of best window

### 🚨 **Edge Cases to Discuss**
- `k = 0`: No flips allowed
- `k ≥ number of zeros`: Can flip all zeros
- All ones or all zeros
- Single element array

---

## 🔄 Alternative Approaches

### Approach 1: Brute Force (Not Recommended)
```python
# O(n²) - Check all subarrays
def longestOnes_bruteforce(nums, k):
    max_len = 0
    for i in range(len(nums)):
        zero_count = 0
        for j in range(i, len(nums)):
            if nums[j] == 0:
                zero_count += 1
            if zero_count <= k:
                max_len = max(max_len, j - i + 1)
            else:
                break
    return max_len
```

### Why Sliding Window is Better:
- ✅ O(n) vs O(n²) time complexity
- ✅ Processes each element exactly once in optimal path
- ✅ More intuitive for window-based problems

---

## 🎪 Related Problems

Once you master this pattern, try these similar problems:

1. **Longest Substring Without Repeating Characters** (LeetCode 3)
2. **Minimum Window Substring** (LeetCode 76)  
3. **Longest Substring with At Most K Distinct Characters** (LeetCode 340)
4. **Max Consecutive Ones** (LeetCode 485)
5. **Max Consecutive Ones II** (LeetCode 487)

---

## 🏆 Key Takeaways

> **Pattern Recognition**: When you see "maximum/minimum subarray/substring with constraint", think **Sliding Window**

> **Two Pointers**: Use left and right pointers to maintain a valid window

> **Constraint Tracking**: Keep count of constraint violations (zeros in this case)

> **Greedy Expansion**: Always try to expand the window, only contract when necessary

---

## 📝 Summary Checklist

- [ ] Understand the problem: find longest subarray with ≤ k zeros
- [ ] Recognize sliding window pattern
- [ ] Implement two-pointer technique with constraint tracking  
- [ ] Analyze time/space complexity
- [ ] Test with examples and edge cases
- [ ] Explain approach clearly in interview setting

**Happy Coding! 🚀**
