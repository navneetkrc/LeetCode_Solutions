# 🚀 LeetCode Interview Preparation Guide
**Big Tech Company Questions from Last 6 Months**

## 📋 Table of Contents
- [Overview](#overview)
- [Problem Categories by Pattern](#problem-categories-by-pattern)
- [Detailed Solutions](#detailed-solutions)
- [Quick Reference](#quick-reference)

## Overview
This document contains **28 frequently asked LeetCode problems** from major tech companies in the last 6 months, organized by patterns with interview-ready code solutions.

## Problem Categories by Pattern

### 🏗️ **Design**
- 146. LRU Cache (Medium)

### 🗺️ **Hash Map/Table**
- 1. Two Sum (Easy)
- 128. Longest Consecutive Sequence (Medium)

### 🔍 **Binary Search**
- 33. Search in Rotated Sorted Array (Medium)
- 4. Median of Two Sorted Arrays (Hard)

### 📚 **Stack**
- 20. Valid Parentheses (Easy)

### 🪟 **Sliding Window**
- 3. Longest Substring Without Repeating Characters (Medium)
- 76. Minimum Window Substring (Hard)

### 📅 **Intervals**
- 56. Merge Intervals (Medium)

### 🔄 **Backtracking**
- 22. Generate Parentheses (Medium)

### 🌊 **BFS/Matrix Traversal**
- 994. Rotting Oranges (Medium)
- 200. Number of Islands (Medium)

### 💡 **Dynamic Programming**
- 139. Word Break (Medium)
- 91. Decode Ways (Medium)
- 1143. Longest Common Subsequence (Medium)

### 🌳 **Tree/DFS**
- 543. Diameter of Binary Tree (Easy)
- 106. Construct Binary Tree from Inorder and Postorder Traversal (Medium)
- 116. Populating Next Right Pointers in Each Node (Medium)

### 🏔️ **Heap/Priority Queue**
- 347. Top K Frequent Elements (Medium)

### ↔️ **Two Pointers**
- 42. Trapping Rain Water (Hard)
- 75. Sort Colors (Medium)

### 🔢 **Matrix**
- 73. Set Matrix Zeroes (Medium)
- 498. Diagonal Traverse (Medium)
- 1329. Sort the Matrix Diagonally (Medium)
- 54. Spiral Matrix (Medium)

---

## Detailed Solutions

### 1️⃣ Two Sum
**🏷️ Tags:** `Array` `Hash Table`  
**⏱️ Time:** O(n) | **💾 Space:** O(n)

```python
def twoSum(nums, target):
    """
    Find two numbers that add up to target
    """
    num_map = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i
    
    return []

# Interview Tips:
# - Always ask about duplicate numbers
# - Mention brute force O(n²) first, then optimize
# - Handle edge cases (empty array, no solution)
```

### 3️⃣ Longest Substring Without Repeating Characters
**🏷️ Tags:** `Hash Table` `String` `Sliding Window`  
**⏱️ Time:** O(n) | **💾 Space:** O(min(m,n))

```python
def lengthOfLongestSubstring(s):
    """
    Sliding window approach with character frequency tracking
    """
    char_count = {}
    left = 0
    max_length = 0
    
    for right, char in enumerate(s):
        char_count[char] = char_count.get(char, 0) + 1
        
        # Shrink window if duplicate found
        while char_count[char] > 1:
            left_char = s[left]
            char_count[left_char] -= 1
            left += 1
        
        max_length = max(max_length, right - left + 1)
    
    return max_length

# Key Points:
# - Maintain window with unique characters
# - Use hash map for O(1) lookup
# - Update max_length on each valid window
```

### 20. Valid Parentheses
**🏷️ Tags:** `String` `Stack`  
**⏱️ Time:** O(n) | **💾 Space:** O(n)

```python
def isValid(s):
    """
    Use stack to match opening and closing brackets
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:  # Closing bracket
            if not stack or stack.pop() != mapping[char]:
                return False
        else:  # Opening bracket
            stack.append(char)
    
    return not stack

# Remember:
# - Stack for nested structures
# - Use dictionary for clean mapping
# - Check empty stack before popping
```

### 22. Generate Parentheses
**🏷️ Tags:** `String` `Dynamic Programming` `Backtracking`  
**⏱️ Time:** O(4^n / √n) | **💾 Space:** O(4^n / √n)

```python
def generateParenthesis(n):
    """
    Backtracking with pruning for valid combinations
    """
    result = []
    
    def backtrack(current, open_count, close_count):
        # Base case: valid combination found
        if len(current) == 2 * n:
            result.append(current)
            return
        
        # Add opening bracket if possible
        if open_count < n:
            backtrack(current + '(', open_count + 1, close_count)
        
        # Add closing bracket if valid
        if close_count < open_count:
            backtrack(current + ')', open_count, close_count + 1)
    
    backtrack('', 0, 0)
    return result

# Backtracking Pattern:
# - Use constraints for pruning
# - Build solution incrementally
# - Backtrack on invalid paths
```

### 33. Search in Rotated Sorted Array
**🏷️ Tags:** `Array` `Binary Search`  
**⏱️ Time:** O(log n) | **💾 Space:** O(1)

```python
def search(nums, target):
    """
    Modified binary search for rotated array
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Check which half is sorted
        if nums[left] <= nums[mid]:  # Left half sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:  # Right half sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1

# Key Insight:
# - One half is always sorted in rotated array
# - Check which half contains target
# - Standard binary search on correct half
```

### 42. Trapping Rain Water
**🏷️ Tags:** `Array` `Two Pointers` `Dynamic Programming` `Stack` `Monotonic Stack`  
**⏱️ Time:** O(n) | **💾 Space:** O(1)

```python
def trap(height):
    """
    Two pointers approach for optimal space
    """
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    water = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]
            right -= 1
    
    return water

# Multiple Solutions:
# - Brute force: O(n²)
# - DP with pre-computation: O(n) time, O(n) space
# - Two pointers: O(n) time, O(1) space (optimal)
```

### 56. Merge Intervals
**🏷️ Tags:** `Array` `Sorting`  
**⏱️ Time:** O(n log n) | **💾 Space:** O(n)

```python
def merge(intervals):
    """
    Sort by start time, then merge overlapping intervals
    """
    if not intervals:
        return []
    
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    
    for current in intervals[1:]:
        last = merged[-1]
        
        # Check for overlap
        if current[0] <= last[1]:
            # Merge intervals
            last[1] = max(last[1], current[1])
        else:
            # No overlap, add new interval
            merged.append(current)
    
    return merged

# Pattern:
# - Sort by start time for easier comparison
# - Compare each interval with last merged
# - Merge if overlapping, else add new
```

### 76. Minimum Window Substring
**🏷️ Tags:** `Hash Table` `String` `Sliding Window`  
**⏱️ Time:** O(|s| + |t|) | **💾 Space:** O(|s| + |t|)

```python
def minWindow(s, t):
    """
    Sliding window with character frequency tracking
    """
    if not s or not t or len(s) < len(t):
        return ""
    
    # Count characters in t
    t_count = {}
    for char in t:
        t_count[char] = t_count.get(char, 0) + 1
    
    left = 0
    min_len = float('inf')
    min_start = 0
    required = len(t_count)
    formed = 0
    window_count = {}
    
    for right in range(len(s)):
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        if char in t_count and window_count[char] == t_count[char]:
            formed += 1
        
        # Contract window if valid
        while formed == required:
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_start = left
            
            left_char = s[left]
            window_count[left_char] -= 1
            if left_char in t_count and window_count[left_char] < t_count[left_char]:
                formed -= 1
            
            left += 1
    
    return "" if min_len == float('inf') else s[min_start:min_start + min_len]
```

### 139. Word Break
**🏷️ Tags:** `Hash Table` `String` `Dynamic Programming` `Trie` `Memoization`  
**⏱️ Time:** O(n² × m) | **💾 Space:** O(n)

```python
def wordBreak(s, wordDict):
    """
    DP approach: dp[i] = can s[0:i] be segmented
    """
    word_set = set(wordDict)
    dp = [False] * (len(s) + 1)
    dp[0] = True  # Empty string
    
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[len(s)]

# DP Pattern:
# - dp[i] represents if s[0:i] can be broken
# - Check all possible splits at each position
# - Use set for O(1) word lookup
```

### 146. LRU Cache
**🏷️ Tags:** `Hash Table` `Linked List` `Design` `Doubly-Linked List`  
**⏱️ Time:** O(1) for get/put | **💾 Space:** O(capacity)

```python
class Node:
    def __init__(self, key=0, val=0):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # key -> node
        
        # Dummy head and tail for easy insertion/deletion
        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def _add_node(self, node):
        """Add node right after head"""
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def _remove_node(self, node):
        """Remove an existing node"""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node
    
    def _move_to_head(self, node):
        """Move node to head (mark as recently used)"""
        self._remove_node(node)
        self._add_node(node)
    
    def get(self, key):
        node = self.cache.get(key)
        if not node:
            return -1
        
        # Move to head (recently used)
        self._move_to_head(node)
        return node.val
    
    def put(self, key, value):
        node = self.cache.get(key)
        
        if not node:
            new_node = Node(key, value)
            
            if len(self.cache) >= self.capacity:
                # Remove LRU (tail.prev)
                last_node = self.tail.prev
                self._remove_node(last_node)
                del self.cache[last_node.key]
            
            self._add_node(new_node)
            self.cache[key] = new_node
        else:
            # Update existing
            node.val = value
            self._move_to_head(node)

# Design Pattern:
# - Hash map for O(1) access
# - Doubly linked list for O(1) insertion/deletion
# - Dummy nodes to simplify edge cases
```

### 200. Number of Islands
**🏷️ Tags:** `Array` `Depth-First Search` `Breadth-First Search` `Union Find` `Matrix`  
**⏱️ Time:** O(m × n) | **💾 Space:** O(m × n)

```python
def numIslands(grid):
    """
    DFS to explore and mark visited islands
    """
    if not grid or not grid[0]:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    islands = 0
    
    def dfs(r, c):
        # Bounds check and water/visited check
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            grid[r][c] == '0'):
            return
        
        # Mark as visited
        grid[r][c] = '0'
        
        # Explore 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                islands += 1
                dfs(r, c)  # Mark entire island
    
    return islands

# Alternative: BFS using queue
# Alternative: Union-Find for dynamic connectivity
```

### 347. Top K Frequent Elements
**🏷️ Tags:** `Array` `Hash Table` `Divide and Conquer` `Sorting` `Heap (Priority Queue)` `Bucket Sort` `Counting` `Quickselect`  
**⏱️ Time:** O(n log k) | **💾 Space:** O(n + k)

```python
import heapq
from collections import Counter

def topKFrequent(nums, k):
    """
    Min heap approach for optimal space when k << n
    """
    count = Counter(nums)
    heap = []
    
    for num, freq in count.items():
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:
            heapq.heappop(heap)
    
    return [num for freq, num in heap]

# Alternative O(n) solution using bucket sort:
def topKFrequentBucket(nums, k):
    count = Counter(nums)
    bucket = [[] for _ in range(len(nums) + 1)]
    
    for num, freq in count.items():
        bucket[freq].append(num)
    
    result = []
    for i in range(len(bucket) - 1, -1, -1):
        result.extend(bucket[i])
        if len(result) >= k:
            break
    
    return result[:k]
```

### 994. Rotting Oranges
**🏷️ Tags:** `Array` `Breadth-First Search` `Matrix`  
**⏱️ Time:** O(m × n) | **💾 Space:** O(m × n)

```python
from collections import deque

def orangesRotting(grid):
    """
    BFS for simultaneous multi-source spreading
    """
    if not grid or not grid[0]:
        return -1
    
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_count = 0
    
    # Find all initially rotten oranges and count fresh ones
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_count += 1
    
    if fresh_count == 0:
        return 0
    
    minutes = 0
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    while queue:
        minutes += 1
        
        # Process all oranges that rot in this minute
        for _ in range(len(queue)):
            r, c = queue.popleft()
            
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                
                if (0 <= nr < rows and 0 <= nc < cols and 
                    grid[nr][nc] == 1):
                    grid[nr][nc] = 2
                    fresh_count -= 1
                    queue.append((nr, nc))
        
        if fresh_count == 0:
            return minutes
    
    return -1

# BFS Pattern:
# - Use queue for level-by-level processing
# - Track time using queue size
# - Multi-source BFS for simultaneous spreading
```

---

## Quick Reference

### 🎯 Most Common Patterns
1. **Hash Map** - O(1) lookup for complements/frequency
2. **Two Pointers** - Opposite ends, same direction, or sliding window
3. **Binary Search** - Sorted arrays, search space reduction
4. **DFS/BFS** - Tree/graph traversal, connected components
5. **Dynamic Programming** - Optimal substructure, overlapping subproblems
6. **Stack** - LIFO for nested structures, monotonic for next greater
7. **Sliding Window** - Subarray/substring problems with constraints

### 📊 Time Complexity Goals
- **O(1)**: Hash operations, cache access
- **O(log n)**: Binary search, heap operations
- **O(n)**: Single pass, hash map building
- **O(n log n)**: Sorting-based solutions
- **O(n²)**: Nested loops (try to optimize)

### 🧠 Interview Strategy
1. **Clarify requirements** (edge cases, constraints)
2. **Start with brute force** (shows understanding)
3. **Optimize step by step** (multiple solutions)
4. **Discuss trade-offs** (time vs space)
5. **Code clean solution** (readable, maintainable)
6. **Test with examples** (edge cases, normal cases)

### 🏷️ Tag Priority for Big Tech
1. **Array & String** (most common)
2. **Hash Table** (optimization technique)
3. **Binary Search** (efficiency)
4. **Tree & Graph** (system design relevance)
5. **Dynamic Programming** (algorithmic thinking)
6. **Design** (system architecture)

---

*💡 Pro Tip: Practice these patterns until you can recognize them instantly. Most interview problems combine 2-3 patterns!*
