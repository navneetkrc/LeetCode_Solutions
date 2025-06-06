# 🏃‍♂️ Jump Game - LeetCode Problem #55

## 📋 Problem Statement

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your **maximum jump length** at that position.

Return `true` if you can reach the last index, or `false` otherwise.

---

## 🎯 Examples

### Example 1: ✅ Successful Jump
```
Input: nums = [2,3,1,1,4]
Output: true

Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Visual representation:
Index:  0  1  2  3  4
Value: [2, 3, 1, 1, 4]
        ↓  ↓        ↓
       Start → → → End
```

### Example 2: ❌ Impossible Jump
```
Input: nums = [3,2,1,0,4]
Output: false

Explanation: You will always arrive at index 3 no matter what. 
Its maximum jump length is 0, which makes it impossible to reach the last index.

Visual representation:
Index:  0  1  2  3  4
Value: [3, 2, 1, 0, 4]
        ↓  ↓  ↓  ⚠️  ❌
       Start → → STUCK! Can't reach index 4
```

---

## 🧠 Solution Approach: Greedy Algorithm

The key insight is to use a **greedy approach** - at each position, we keep track of the farthest index we can possibly reach. If we ever encounter a position that's beyond our reach, we know it's impossible to complete the journey.

### 🔍 Algorithm Walkthrough

1. **Initialize** the maximum reachable position as 0
2. **Iterate** through each position in the array
3. **Check** if current position is reachable
4. **Update** the maximum reachable position greedily
5. **Return** true if we complete the iteration

---

## 💻 Enhanced Solution Code

```python
from typing import List

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # Farthest index we can reach at any point
        farthest = 0
        target = len(nums) - 1

        for i in range(len(nums)):
            # If the current index is beyond what we can reach, we fail
            if i > farthest:
                return False
            
            # Greedily update the farthest reachable index
            farthest = max(farthest, i + nums[i])

            #stop early if we already reached goal
            if farthest >= target:
                 return True
        
        # If we finish the loop, we can reach the end
        return True

```

---

## 🎨 Step-by-Step Visualization

Let's trace through **Example 1**: `nums = [2,3,1,1,4]`

| Step | Position | Value | Max Jump From Here | Max Reachable | Status |
|------|----------|-------|-------------------|---------------|---------|
| 1    | 0        | 2     | 0 + 2 = 2        | max(0, 2) = 2 | ✅ Continue |
| 2    | 1        | 3     | 1 + 3 = 4        | max(2, 4) = 4 | ✅ Can reach end! |
| 3    | 2        | 1     | 2 + 1 = 3        | max(4, 3) = 4 | ✅ Continue |
| 4    | 3        | 1     | 3 + 1 = 4        | max(4, 4) = 4 | ✅ Continue |

**Result**: `max_reachable_position = 4 >= target_index = 4` → **True** ✅

---

Let's trace through **Example 2**: `nums = [3,2,1,0,4]`

| Step | Position | Value | Max Jump From Here | Max Reachable | Status |
|------|----------|-------|-------------------|---------------|---------|
| 1    | 0        | 3     | 0 + 3 = 3        | max(0, 3) = 3 | ✅ Continue |
| 2    | 1        | 2     | 1 + 2 = 3        | max(3, 3) = 3 | ✅ Continue |
| 3    | 2        | 1     | 2 + 1 = 3        | max(3, 3) = 3 | ✅ Continue |
| 4    | 3        | 0     | 3 + 0 = 3        | max(3, 3) = 3 | ⚠️ Stuck at 3 |

**Result**: `max_reachable_position = 3 < target_index = 4` → **False** ❌

---

## 🚀 Key Optimizations

### 1. **Early Termination**
```python
# If we can already reach the end, stop processing
if max_reachable_position >= target_index:
    return True
```

### 2. **Efficient Loop Range**
```python
# Only iterate to len(nums) - 1 since we don't need to jump FROM the last position
for current_position in range(len(nums) - 1):
```

### 3. **Greedy Choice**
```python
# Always take the maximum reachable position - this gives us the best chance
max_reachable_position = max(max_reachable_position, jump_range_from_current)
```

---

## 📊 Complexity Analysis

| Metric | Value | Explanation |
|--------|-------|-------------|
| **Time Complexity** | `O(n)` | Single pass through the array |
| **Space Complexity** | `O(1)` | Only using constant extra variables |
| **Best Case** | `O(1)` | When first element can reach the end |
| **Worst Case** | `O(n)` | Must check every position |

---

## 🧪 Alternative Approaches

### Approach 1: Dynamic Programming (Less Efficient)
```python
def canJump_dp(self, nums: List[int]) -> bool:
    """
    DP approach: dp[i] = True if position i is reachable
    Time: O(n²), Space: O(n)
    """
    n = len(nums)
    dp = [False] * n
    dp[0] = True
    
    for i in range(n):
        if dp[i]:
            for j in range(1, nums[i] + 1):
                if i + j < n:
                    dp[i + j] = True
    
    return dp[n - 1]
```

### Approach 2: Backwards Greedy
```python
def canJump_backwards(self, nums: List[int]) -> bool:
    """
    Start from the end and work backwards
    Time: O(n), Space: O(1)
    """
    last_good_position = len(nums) - 1
    
    for i in range(len(nums) - 2, -1, -1):
        if i + nums[i] >= last_good_position:
            last_good_position = i
    
    return last_good_position == 0
```

---

## ✅ Test Cases

```python
def test_jump_game():
    solution = Solution()
    
    # Test Case 1: Basic positive case
    assert solution.canJump([2,3,1,1,4]) == True
    
    # Test Case 2: Basic negative case  
    assert solution.canJump([3,2,1,0,4]) == False
    
    # Test Case 3: Single element
    assert solution.canJump([0]) == True
    
    # Test Case 4: Can't move from start
    assert solution.canJump([0,1]) == False
    
    # Test Case 5: Large jumps
    assert solution.canJump([1,1,1,1,1]) == True
    
    # Test Case 6: Exact reach
    assert solution.canJump([1,1,1,1,0]) == True
    
    print("All test cases passed! ✅")

test_jump_game()
```

---

## 🎯 Key Takeaways

1. **Greedy is Optimal**: For this problem, the greedy approach gives us the optimal solution
2. **Track Maximum Reach**: Always maintain the farthest position we can reach
3. **Early Detection**: Stop as soon as we find an unreachable position
4. **Simple but Powerful**: The algorithm is elegant in its simplicity yet highly efficient

> 💡 **Pro Tip**: This problem is a classic example of how greedy algorithms can be more efficient than dynamic programming when the greedy choice property holds!
