<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Python Coding Interview Cheatsheet (Expanded)

This comprehensive cheatsheet includes top coding patterns, corresponding **LeetCode practice problem types**, and critical points to **showcase** during interviews to meet **interviewer expectations**. Use clear variable names, approach each problem confidently, communicate trade-offs, and demonstrate your understanding of the underlying algorithms.

***

## 1. Two Pointers: One Input, Opposite Ends

```python
def has_pair_with_sum(sorted_array, target_sum):
    left = 0
    right = len(sorted_array) - 1
    while left < right:
        current_sum = sorted_array[left] + sorted_array[right]
        if current_sum == target_sum:
            return True
        elif current_sum < target_sum:
            left += 1
        else:
            right -= 1
    return False
```

- **LeetCode Types**: Two Sum II (167), Valid Palindrome (125), Trapping Rain Water (42), Container With Most Water (11), Reverse String (344)
- **Showcase**: O(n) optimization over brute-force, why the array must be sorted, edge case handling (duplicates, negatives)

***

## 2. Two Pointers: Two Inputs, Exhaust Both

```python
def merge_sorted_arrays(array_one, array_two):
    pointer_one, pointer_two = 0, 0
    merged_array = []
    while pointer_one < len(array_one) and pointer_two < len(array_two):
        if array_one[pointer_one] < array_two[pointer_two]:
            merged_array.append(array_one[pointer_one])
            pointer_one += 1
        else:
            merged_array.append(array_two[pointer_two])
            pointer_two += 1
    merged_array += array_one[pointer_one:]
    merged_array += array_two[pointer_two:]
    return merged_array
```

- **LeetCode Types**: Merge Sorted Array (88), Intersection of Two Arrays II (350), Median of Two Sorted Arrays (4)
- **Showcase**: Linear merge, minimizing extra space, correctness for repeated elements

***

## 3. Sliding Window

```python
def max_subarray_sum(nums, window_size):
    max_sum = curr_sum = sum(nums[:window_size])
    for end in range(window_size, len(nums)):
        curr_sum += nums[end] - nums[end - window_size]
        max_sum = max(max_sum, curr_sum)
    return max_sum
```

- **LeetCode Types**: Maximum Average Subarray I (643), Longest Substring Without Repeating Characters (3), Minimum Size Subarray Sum (209), Longest Repeating Character Replacement (424)
- **Showcase**: O(n) window movement, difference between fixed and variable windows, use case selection

***

## 4. Build a Prefix Sum

```python
def get_prefix_sums(input_array):
    prefix_sums = [^0]
    for number in input_array:
        prefix_sums.append(prefix_sums[-1] + number)
    return prefix_sums
```

- **LeetCode Types**: Range Sum Query - Immutable (303), Subarray Sum Equals K (560), Find Pivot Index (724)
- **Showcase**: Preprocessing for O(1) or O(n) queries, memory trade-off, common subarray tricks

***

## 5. Efficient String Building

```python
def join_words(word_list):
    return ''.join(word_list)
```

- **LeetCode Types**: Reverse Words in a String (151), Group Anagrams (49), Longest Common Prefix (14)
- **Showcase**: Efficient concatenation, why `+=` is slower for large lists

***

## 6. Linked List: Fast and Slow Pointer

```python
def has_cycle(head_node):
    fast_pointer = slow_pointer = head_node
    while fast_pointer and fast_pointer.next:
        slow_pointer = slow_pointer.next
        fast_pointer = fast_pointer.next.next
        if slow_pointer == fast_pointer:
            return True
    return False
```

- **LeetCode Types**: Linked List Cycle (141), Middle of the Linked List (876), Linked List Cycle II (142)
- **Showcase**: O(1) space and why fast/slow works, cycle detection, finding midpoint

***

## 7. Reversing a Linked List

```python
def reverse_linked_list(head_node):
    previous_node = None
    current_node = head_node
    while current_node:
        next_node = current_node.next
        current_node.next = previous_node
        previous_node = current_node
        current_node = next_node
    return previous_node
```

- **LeetCode Types**: Reverse Linked List (206), Palindrome Linked List (234), Add Two Numbers II (445)
- **Showcase**: In-place reversal, pointer management, recursion vs iteration

***

## 8. Find Number of Subarrays that Fit Criteria

```python
def count_subarrays_with_sum(nums, target_sum):
    from collections import defaultdict
    prefix_sum_count = defaultdict(int)
    prefix_sum_count = 1
    curr_sum = count = 0
    for num in nums:
        curr_sum += num
        count += prefix_sum_count[curr_sum - target_sum]
        prefix_sum_count[curr_sum] += 1
    return count
```

- **LeetCode Types**: Subarray Sum Equals K (560), Minimum Size Subarray Sum (209)
- **Showcase**: Use of hashmap, difference between prefix sum (counting) and sliding window (finding)

***

## 9. Monotonic Increasing Stack

```python
def next_smaller_elements(nums):
    stack = []
    result = [-1] * len(nums)
    for idx, value in enumerate(nums):
        while stack and nums[stack[-1]] > value:
            result[stack.pop()] = value
        stack.append(idx)
    return result
```

- **LeetCode Types**: Next Greater Element I/II (496/503), Daily Temperatures (739), Largest Rectangle in Histogram (84)
- **Showcase**: O(n) stack operations, trace how stack changes, where monotonicity is used

***

## 10. Binary Tree: DFS (Recursive)

```python
def dfs_inorder_recursive(node):
    if not node:
        return []
    return dfs_inorder_recursive(node.left) + [node.val] + dfs_inorder_recursive(node.right)
```

- **LeetCode Types**: Binary Tree Inorder Traversal (94), Maximum Depth of Binary Tree (104)
- **Showcase**: Recursion tree, pre/in/post-order difference, recursion base case

***

## 11. Binary Tree: DFS (Iterative)

```python
def dfs_inorder_iterative(root_node):
    stack, result = [], []
    current = root_node
    while stack or current:
        while current:
            stack.append(current)
            current = current.left
        current = stack.pop()
        result.append(current.val)
        current = current.right
    return result
```

- **LeetCode Types**: Binary Tree Inorder Traversal (94), Binary Tree Pre/Postorder Traversal (144/145)
- **Showcase**: Simulating recursion with a stack, order of visiting nodes

***

## 12. Binary Tree: BFS

```python
from collections import deque
def bfs_level_order(root_node):
    results = []
    queue = deque([root_node])
    while queue:
        node = queue.popleft()
        if node:
            results.append(node.val)
            queue.append(node.left)
            queue.append(node.right)
    return results
```

- **LeetCode Types**: Binary Tree Level Order Traversal (102), Symmetric Tree (101)
- **Showcase**: Level-wise traversal, queue mechanics, real world use (shortest path, finding minimum)

***

## 13. Graph: DFS (Recursive)

```python
def dfs_recursive(graph, start_node, visited=None):
    if visited is None:
        visited = set()
    visited.add(start_node)
    for neighbor in graph[start_node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
    return visited
```

- **LeetCode Types**: Number of Connected Components in an Undirected Graph (323), Graph Valid Tree (261), Surrounded Regions (130)
- **Showcase**: Visited set to avoid cycles, graph exploration, recursion depth

***

## 14. Graph: DFS (Iterative)

```python
def dfs_iterative(graph, start_node):
    stack, visited = [start_node], set()
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            stack.extend(n for n in graph[node] if n not in visited)
    return visited
```

- **LeetCode Types**: Islands (DFS) (200), Clone Graph (133)
- **Showcase**: Stack for exploration, difference vs recursive (stack overflow avoidance)

***

## 15. Graph: BFS

```python
from collections import deque
def bfs_graph(graph, start_node):
    queue, visited = deque([start_node]), set([start_node])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return visited
```

- **LeetCode Types**: Shortest Path in Binary Matrix (1091), Word Ladder (127)
- **Showcase**: Level-order in graphs, shortest path finding, queue logic

***

## 16. Find Top K Elements with Heap

```python
import heapq
def find_top_k_largest(nums, k):
    return heapq.nlargest(k, nums)
```

- **LeetCode Types**: Top K Frequent Elements (347), Kth Largest Element in Array (215), Find Median from Data Stream (295)
- **Showcase**: Heap size, max vs min heap, stream processing

***

## 17. Binary Search

```python
def binary_search(sorted_array, target):
    left, right = 0, len(sorted_array) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if sorted_array[mid] == target:
            return mid
        elif sorted_array[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

- **LeetCode Types**: Binary Search (704), Search Insert Position (35), Search in Rotated Sorted Array (33)
- **Showcase**: Why O(log n), preventing overflow, loop invariants

***

## 18. Binary Search: Left-most Insertion Point (Lower Bound)

```python
def leftmost_insertion(sorted_array, target):
    left, right = 0, len(sorted_array)
    while left < right:
        mid = (left + right) // 2
        if sorted_array[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

- **LeetCode Types**: Find First Position of Element in Sorted Array (34), Lower Bound variations
- **Showcase**: Lower bound utility, insertion points, handling duplicates

***

## 19. Binary Search: Right-most Insertion Point (Upper Bound)

```python
def rightmost_insertion(sorted_array, target):
    left, right = 0, len(sorted_array)
    while left < right:
        mid = (left + right) // 2
        if sorted_array[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left
```

- **LeetCode Types**: Find Last Position of Element in Sorted Array (34), Upper Bound
- **Showcase**: Comparison to left-most, why boundaries differ

***

## 20. Binary Search for Greedy Problems (Max/Min Value)

```python
def min_capacity(nums, days):
    left, right = max(nums), sum(nums)
    while left < right:
        mid = (left + right) // 2
        required_days, running_sum = 1, 0
        for num in nums:
            running_sum += num
            if running_sum > mid:
                required_days += 1
                running_sum = num
        if required_days > days:
            left = mid + 1
        else:
            right = mid
    return left
```

- **LeetCode Types**: Capacity To Ship Packages Within D Days (1011), Split Array Largest Sum (410)
- **Showcase**: Search space understanding, conversion of greedy solution to binary search framework

***

## 21. Backtracking

```python
def permutations(nums):
    def backtrack(path, used, result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            if not used[i]:
                used[i] = True
                path.append(nums[i])
                backtrack(path, used, result)
                path.pop()
                used[i] = False
    result = []
    backtrack([], [False] * len(nums), result)
    return result
```

- **LeetCode Types**: Permutations (46), Subsets (78), N-Queens (51), Word Search (79), Combination Sum (39)
- **Showcase**: State management (path, used), pruning/constraints, recursion tree explanation

***

## 22. Dynamic Programming: Top-Down Memoization

```python
def fibonacci(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo)
    return memo[n]
```

- **LeetCode Types**: Climbing Stairs (70), House Robber (198), Coin Change (322), Longest Palindromic Substring (5), Edit Distance (72)
- **Showcase**: Recursion, overlapping subproblems, memo usage, brute-force to DP optimization

***

## 23. Build a Trie

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
```

- **LeetCode Types**: Implement Trie (208), Word Search II (212), Replace Words (648), Add and Search Word (211)
- **Showcase**: Prefix efficiency vs hashmap, trie operation complexity, applications (autocomplete, spell-checking)

***

## 24. Dijkstra's Algorithm

```python
import heapq
def dijkstra(graph, start_node):
    distances = {node: float('inf') for node in graph}
    distances[start_node] = 0
    heap = [(0, start_node)]
    while heap:
        curr_dist, node = heapq.heappop(heap)
        if curr_dist > distances[node]:
            continue
        for neighbor, weight in graph[node]:
            new_dist = curr_dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(heap, (new_dist, neighbor))
    return distances
```

- **LeetCode Types**: Network Delay Time (743), Path With Minimum Effort (1631), Cheapest Flights Within K Stops (787)
- **Showcase**: Priority queue usage, why Dijkstra works for non-negative weights, optimality guarantees

***

## 25. Prim's Algorithm

```python
import heapq
def prim_minimum_spanning_tree(graph, start_node):
    visited = set()
    min_heap = [(0, start_node)]
    total_weight = 0
    while min_heap:
        weight, node = heapq.heappop(min_heap)
        if node not in visited:
            visited.add(node)
            total_weight += weight
            for neighbor, edge_weight in graph[node]:
                if neighbor not in visited:
                    heapq.heappush(min_heap, (edge_weight, neighbor))
    return total_weight
```

- **LeetCode Types**: Min Cost to Connect All Points (1584), Connecting Cities With Minimum Cost (1135)
- **Showcase**: Greedy MST selection, managing visited nodes, heap performance

***

## 26. Kruskal's Algorithm

```python
def kruskal_minimum_spanning_tree(num_nodes, edges):
    edges.sort(key=lambda x: x[^2])  # (node_a, node_b, edge_weight)
    parent = list(range(num_nodes))

    def find(node):
        while parent[node] != node:
            parent[node] = parent[parent[node]]
            node = parent[node]
        return node

    mst_weight = 0
    for node_a, node_b, edge_weight in edges:
        root_a, root_b = find(node_a), find(node_b)
        if root_a != root_b:
            parent[root_b] = root_a
            mst_weight += edge_weight
    return mst_weight
```

- **LeetCode Types**: Min Cost to Connect All Points (1584), Connecting Cities With Minimum Cost (1135)
- **Showcase**: Union-find efficiency, edge sorting, cycle prevention

***

## Interviewer Expectations

- **Communicate trade-offs**: Clearly discuss time and space complexity, and why you choose a certain approach.
- **Demonstrate pattern recognition**: Relate the problem to common patterns you've practiced.
- **Explain edge case handling**: Discuss boundary conditions and correctness.
- **Show process clarity**: Narrate your approach, state your invariants, and test with examples.
- **Optimize when possible**: Identify brute-force and show how your approach improves upon it.
- **Write clean code**: Use meaningful variable names, avoid magic numbers, modularize logic.

***

**Practicing these LeetCode patterns with conscious interview expectations will set you apart. Use this sheet during mock interviews for reference, and always verbalize your thought process.**
<span style="display:none">[^1][^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^3][^30][^31][^32][^4][^5][^6][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://www.youtube.com/watch?v=r_mVgmc89_U

[^2]: https://www.youtube.com/watch?v=y2d0VHdvfdc

[^3]: https://algomap.io/problems/binary-search

[^4]: https://www.youtube.com/watch?v=QzZ7nmouLTI

[^5]: https://www.geeksforgeeks.org/dsa/window-sliding-technique/

[^6]: https://www.reddit.com/r/leetcode/comments/114ak49/struggling_with_binary_search/

[^7]: https://www.youtube.com/watch?v=6lX7x1RcLvg

[^8]: https://www.youtube.com/watch?v=Mf9C6vDxhzk

[^9]: https://www.youtube.com/watch?v=UKXsZBHmmFk

[^10]: https://www.geeksforgeeks.org/dsa/two-pointers-technique/

[^11]: https://www.youtube.com/watch?v=o7qCzQOmdBA

[^12]: https://blogs.oregonstate.edu/codingpatterns/2022/10/24/fast-slow-pointers/

[^13]: https://www.youtube.com/watch?v=cS-198wtfj0

[^14]: https://www.reddit.com/r/leetcode/comments/1g5xs2d/𝐌𝐲_%F0%9D%90%87%F0%9D%90%9E%F0%9D%90%9A%F0%9D%90%A9%F0%9D%90%AC_%F0%9D%90%83%F0%9D%90%A8%F0%9D%90%A7%F0%9D%90%AD_%F0%9D%90%8B%F0%9D%90%A2%F0%9D%90%9E_%F0%9D%90%8E%F0%9D%90%AB_%F0%9D%90%87%F0%9D%90%A8%F0%9D%90%B0_%F0%9D%90%93%F0%9D%90%A8_%F0%9D%90%92%F0%9D%90%A8%F0%9D%90%A5%F0%9D%90%AF%F0%9D%90%9E_%F0%9D%90%80%F0%9D%90%A7%F0%9D%90%B2_%F0%9D%90%8F%F0%9D%90%AB%F0%9D%90%A2%F0%9D%90%A8%F0%9D%90%AB%F0%9D%90%A2%F0%9D%90%AD%F0%9D%90%B2/

[^15]: https://emre.me/coding-patterns/fast-slow-pointers/

[^16]: https://www.youtube.com/watch?v=bew9SmOMsDc

[^17]: http://leetcodethehardway.com/tutorials/basic-topics/heap

[^18]: https://www.youtube.com/watch?v=b139yf7Ik-E

[^19]: https://www.reddit.com/r/cscareerquestions/comments/p8jz76/how_to_get_better_at_dfsbfs_leetcode_questions/

[^20]: https://leetcode.com/problem-list/heap-priority-queue/

[^21]: https://leetcode.com/explore/learn/card/linked-list/214/linked-list-two-pointer/

[^22]: https://www.youtube.com/watch?v=p9m2LHBW81M

[^23]: https://www.educative.io/blog/leetcode-dynamic-programming

[^24]: https://www.reddit.com/r/csMajors/comments/l2wdkj/trie_for_leetcode/

[^25]: https://www.youtube.com/watch?v=yTwvZjri-RM

[^26]: https://www.reddit.com/r/leetcode/comments/14o10jd/the_ultimate_dynamic_programming_roadmap/

[^27]: https://www.techinterviewhandbook.org/algorithms/trie/

[^28]: https://www.youtube.com/watch?v=IEPsp-rVbdk

[^29]: https://leetcode.com/problem-list/dynamic-programming/

[^30]: https://www.youtube.com/watch?v=oobqoCJlHA0

[^31]: https://leetcode.com/problem-list/backtracking/

[^32]: https://www.geeksforgeeks.org/dsa/trie-insert-and-search/

