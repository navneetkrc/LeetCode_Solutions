Got it — here’s the **same CodeSignal-style Python Template Sheet** but now with **inline comments** showing **Leetcode problem numbers** where each template applies, so tomorrow you’ll instantly know which template to grab for which problem.

---

## **1. Heap / Priority Queue Templates**

```python
import heapq

# Max Heap (store negative values)
# LC 215 - Kth Largest Element in an Array
# LC 703 - Kth Largest Element in a Stream
def max_heap(nums, k):
    heap = []
    for n in nums:
        heapq.heappush(heap, -n)
        if len(heap) > k:
            heapq.heappop(heap)
    return -heap[0]

# Min Heap (default in Python)
# LC 373 - Find K Pairs with Smallest Sums
def min_heap(nums, k):
    heap = []
    for n in nums:
        heapq.heappush(heap, n)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]

# Top-K Frequent Elements
# LC 347 - Top K Frequent Elements
# LC 692 - Top K Frequent Words
def top_k_frequent(nums, k):
    from collections import Counter
    count = Counter(nums)
    return [item for item, _ in heapq.nlargest(k, count.items(), key=lambda x: x[1])]
```

---

## **2. Sliding Window Templates**

```python
# Fixed-size sliding window
# LC 239 - Sliding Window Maximum
def max_sliding_window(nums, k):
    from collections import deque
    q = deque()
    res = []
    for i, num in enumerate(nums):
        while q and nums[q[-1]] < num:
            q.pop()
        q.append(i)
        if q[0] == i - k:
            q.popleft()
        if i >= k - 1:
            res.append(nums[q[0]])
    return res

# Variable-size sliding window (two-pointer)
# LC 76 - Minimum Window Substring
# LC 3 - Longest Substring Without Repeating Characters
def min_window_substring(s, t):
    from collections import Counter
    need = Counter(t)
    missing = len(t)
    left = start = end = 0
    for right, char in enumerate(s, 1):
        if need[char] > 0:
            missing -= 1
        need[char] -= 1
        if missing == 0:
            while left < right and need[s[left]] < 0:
                need[s[left]] += 1
                left += 1
            if end == 0 or right - left < end - start:
                start, end = left, right
            need[s[left]] += 1
            missing += 1
            left += 1
    return s[start:end]
```

---

## **3. Two-Pointer Templates**

```python
# Two-pointer for sorted arrays
# LC 167 - Two Sum II: Input Array is Sorted
# (also useful after sorting for LC 373, LC 658)
def two_sum_sorted(nums, target):
    l, r = 0, len(nums) - 1
    while l < r:
        s = nums[l] + nums[r]
        if s == target:
            return [l, r]
        elif s < target:
            l += 1
        else:
            r -= 1
    return []

# Two-pointer for unsorted arrays (with sort)
# LC 658 - Find K Closest Elements (with binary search + two-pointer)
def two_sum_unsorted(nums, target):
    nums = sorted(nums)
    return two_sum_sorted(nums, target)
```

---

## **4. Quick Graph/BFS/DFS Template**

```python
from collections import deque

# BFS
# LC 133 - Clone Graph
# LC 207 - Course Schedule
# LC 210 - Course Schedule II
def bfs(start, graph):
    q = deque([start])
    visited = set([start])
    while q:
        node = q.popleft()
        for nei in graph[node]:
            if nei not in visited:
                visited.add(nei)
                q.append(nei)
    return visited

# DFS
# LC 133 - Clone Graph
# LC 207 - Course Schedule
# LC 210 - Course Schedule II
def dfs(node, graph, visited):
    if node in visited:
        return
    visited.add(node)
    for nei in graph[node]:
        dfs(nei, graph, visited)
```

---

## **5. Common Imports & Shortcuts**

```python
import heapq
from collections import Counter, defaultdict, deque
```

---

This way tomorrow, if you see a problem like **692 – Top K Frequent Words**, you’ll instantly know:

> Use the **`top_k_frequent`** heap pattern above, swap `nums` with words.

---

If you want, I can now **merge this template sheet + the 4-hour Leetcode drill plan** into a **single printable PDF** so you can have **one page for patterns, one page for problem sequence** during your last-minute warm-up tomorrow. That would make your prep ultra-efficient.
