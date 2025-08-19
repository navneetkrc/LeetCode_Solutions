# 🐍 Python Interview Cheatsheet (with LeetCode references)

---

## 1. Two Pointers: One Input, Opposite Ends

```python
def is_palindrome(word: str) -> bool:
    left, right = 0, len(word) - 1
    while left < right:
        if word[left] != word[right]:
            return False
        left += 1
        right -= 1
    return True
```

✅ **Key Uses**: Palindrome check, two-sum sorted array, trapping rain water.
📌 **LeetCode**:

* **125. Valid Palindrome**
* **11. Container With Most Water**
* **42. Trapping Rain Water**

---

## 2. Two Pointers: Two Inputs, Exhaust Both

```python
def merge_sorted_lists(list1, list2):
    i, j, merged = 0, 0, []
    while i < len(list1) and j < len(list2):
        if list1[i] < list2[j]:
            merged.append(list1[i]); i += 1
        else:
            merged.append(list2[j]); j += 1
    return merged + list1[i:] + list2[j:]
```

✅ **Key Uses**: Merging, LCS, subsequence check.
📌 **LeetCode**:

* **21. Merge Two Sorted Lists**
* **88. Merge Sorted Array**
* **392. Is Subsequence**

---

## 3. Sliding Window

```python
def longest_unique_substring(s: str) -> int:
    window, left, max_len = {}, 0, 0
    for right, char in enumerate(s):
        if char in window and window[char] >= left:
            left = window[char] + 1
        window[char] = right
        max_len = max(max_len, right - left + 1)
    return max_len
```

✅ **Key Uses**: Longest substring, subarray sums, anagrams.
📌 **LeetCode**:

* **3. Longest Substring Without Repeating Characters**
* **76. Minimum Window Substring**
* **567. Permutation in String**

---

## 4. Prefix Sum

```python
def subarray_sum(nums, k):
    prefix, count, seen = 0, 0, {0: 1}
    for num in nums:
        prefix += num
        count += seen.get(prefix - k, 0)
        seen[prefix] = seen.get(prefix, 0) + 1
    return count
```

✅ **Key Uses**: Range sum queries, subarray sum problems.
📌 **LeetCode**:

* **560. Subarray Sum Equals K**
* **303. Range Sum Query - Immutable**
* **238. Product of Array Except Self**

---

## 5. Efficient String Building

```python
def build_string(words):
    return "".join(words)
```

✅ Use `"".join()` instead of repeated concatenation.
📌 **LeetCode**:

* **415. Add Strings**
* **67. Add Binary**

---

## 6. Linked List: Fast & Slow Pointers

```python
def has_cycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow == fast:
            return True
    return False
```

✅ **Key Uses**: Cycle detection, middle node.
📌 **LeetCode**:

* **141. Linked List Cycle**
* **876. Middle of the Linked List**

---

## 7. Reverse Linked List

```python
def reverse_list(head):
    prev, curr = None, head
    while curr:
        next_node, curr.next = curr.next, prev
        prev, curr = curr, next_node
    return prev
```

📌 **LeetCode**:

* **206. Reverse Linked List**

---

## 8. Count Subarrays with Criteria

```python
def count_subarrays(nums, k):
    prefix, seen, count = 0, {0: 1}, 0
    for num in nums:
        prefix += num
        count += seen.get(prefix - k, 0)
        seen[prefix] = seen.get(prefix, 0) + 1
    return count
```

📌 **LeetCode**:

* **560. Subarray Sum Equals K**
* **1248. Count Number of Nice Subarrays**

---

## 9. Monotonic Increasing Stack

```python
def next_greater(nums):
    stack, result = [], [-1] * len(nums)
    for i, num in enumerate(nums):
        while stack and nums[stack[-1]] < num:
            result[stack.pop()] = num
        stack.append(i)
    return result
```

📌 **LeetCode**:

* **739. Daily Temperatures**
* **496. Next Greater Element I**
* **84. Largest Rectangle in Histogram**

---

## 10. Binary Tree DFS (Recursive)

```python
def dfs_recursive(node):
    if not node: return []
    return dfs_recursive(node.left) + [node.val] + dfs_recursive(node.right)
```

📌 **LeetCode**:

* **94. Binary Tree Inorder Traversal**
* **104. Maximum Depth of Binary Tree**

---

## 11. Binary Tree DFS (Iterative)

```python
def dfs_iterative(root):
    stack, result = [root], []
    while stack:
        node = stack.pop()
        if node:
            result.append(node.val)
            stack.append(node.right)
            stack.append(node.left)
    return result
```

📌 **LeetCode**:

* **144. Binary Tree Preorder Traversal**

---

## 12. Binary Tree BFS

```python
from collections import deque
def bfs(root):
    queue, result = deque([root]), []
    while queue:
        node = queue.popleft()
        if node:
            result.append(node.val)
            queue.extend([node.left, node.right])
    return result
```

📌 **LeetCode**:

* **102. Binary Tree Level Order Traversal**

---

## 13. Graph DFS (Recursive)

```python
def dfs_recursive(graph, node, visited):
    if node in visited: return
    visited.add(node)
    for nei in graph[node]:
        dfs_recursive(graph, nei, visited)
```

📌 **LeetCode**:

* **200. Number of Islands**
* **547. Number of Provinces**

---

## 14. Graph DFS (Iterative)

```python
def dfs_iterative(graph, start):
    stack, visited = [start], set()
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            stack.extend(graph[node])
    return visited
```

---

## 15. Graph BFS

```python
def bfs_graph(graph, start):
    queue, visited = deque([start]), set([start])
    while queue:
        node = queue.popleft()
        for nei in graph[node]:
            if nei not in visited:
                visited.add(nei)
                queue.append(nei)
```

📌 **LeetCode**:

* **133. Clone Graph**
* **127. Word Ladder**

---

## 16. Top K Elements with Heap

```python
import heapq
def top_k(nums, k):
    return heapq.nlargest(k, nums)
```

📌 **LeetCode**:

* **347. Top K Frequent Elements**
* **215. Kth Largest Element in Array**

---

## 17. Binary Search (Classic)

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target: return mid
        elif nums[mid] < target: left = mid + 1
        else: right = mid - 1
    return -1
```

📌 **LeetCode**:

* **704. Binary Search**

---

## 18. Binary Search: Left-Most Insert

```python
import bisect
pos = bisect.bisect_left([1,2,2,3], 2)  # -> 1
```

📌 **LeetCode**:

* **34. Find First and Last Position of Element**

---

## 19. Binary Search: Right-Most Insert

```python
import bisect
pos = bisect.bisect_right([1,2,2,3], 2)  # -> 3
```

---

## 20. Binary Search for Greedy

```python
def minimize_max(nums):
    left, right = min(nums), max(nums)
    while left < right:
        mid = (left + right) // 2
        if can_do(mid):
            right = mid
        else:
            left = mid + 1
    return left
```

📌 **LeetCode**:

* **410. Split Array Largest Sum**
* **875. Koko Eating Bananas**

---

## 21. Backtracking

```python
def backtrack(path, options):
    if not options:
        print(path); return
    for i, choice in enumerate(options):
        backtrack(path+[choice], options[:i]+options[i+1:])
```

📌 **LeetCode**:

* **46. Permutations**
* **39. Combination Sum**

---

## 22. DP: Top-Down Memoization

```python
from functools import lru_cache
@lru_cache(None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)
```

📌 **LeetCode**:

* **70. Climbing Stairs**
* **198. House Robber**

---

## 23. Build a Trie

```python
class TrieNode:
    def __init__(self):
        self.children, self.is_end = {}, False
class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
```

📌 **LeetCode**:

* **208. Implement Trie**
* **211. Add and Search Word**

---

## 24. Dijkstra’s Algorithm

```python
import heapq
def dijkstra(graph, start):
    pq, dist = [(0,start)], {start: 0}
    while pq:
        d, node = heapq.heappop(pq)
        for nei, w in graph[node]:
            new_dist = d + w
            if new_dist < dist.get(nei, float("inf")):
                dist[nei] = new_dist
                heapq.heappush(pq, (new_dist, nei))
    return dist
```

📌 **LeetCode**:

* **743. Network Delay Time**

---

## 25. Prim’s Algorithm (MST)

```python
import heapq
def prim(graph, start):
    visited, edges, mst = set([start]), [(0, start)], 0
    while edges:
        cost, node = heapq.heappop(edges)
        if node not in visited:
            mst += cost
            visited.add(node)
            for nei, w in graph[node]:
                heapq.heappush(edges, (w, nei))
    return mst
```

📌 **LeetCode**:

* **1135. Connecting Cities With Minimum Cost**

---

## 26. Kruskal’s Algorithm (MST with Union-Find)

```python
def kruskal(n, edges):
    parent = list(range(n))
    def find(x):
        while x != parent[x]:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x
    def union(x, y):
        parent[find(x)] = find(y)

    mst = 0
    for w,u,v in sorted(edges):
        if find(u) != find(v):
            union(u,v)
            mst += w
    return mst
```

📌 **LeetCode**:

* **1584. Min Cost to Connect All Points**

---

⚡ This covers **all major coding patterns + LeetCode mappings**.
