<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Python Coding Interview Cheatsheet (with LeetCode Mapping + Interviewer Expectations)

Concise templates with clear variable names, common LeetCode-style topics to practice, and what interviewers typically look for.

***

## 1. Two pointers: one input, opposite ends

Use on sorted arrays to meet a condition (sum, difference).

```python
def has_pair_with_sum(sorted_nums, target_sum):
    left, right = 0, len(sorted_nums) - 1
    while left < right:
        current_sum = sorted_nums[left] + sorted_nums[right]
        if current_sum == target_sum:
            return True
        if current_sum < target_sum:
            left += 1
        else:
            right -= 1
    return False
```

- LeetCode practice: Two Sum II, 3Sum, 3Sum Closest, Valid Palindrome, Reverse Vowels, Remove Duplicates from Sorted Array
- Show the interviewer: sorted precondition, O(n) time/O(1) space, pointer movement rationale, off-by-one handling, duplicates handling strategy.

***

## 2. Two pointers: two inputs, exhaust both

Merge-like sweeps across two sorted lists/arrays.

```python
def merge_sorted_arrays(nums_a, nums_b):
    i, j, merged = 0, 0, []
    while i < len(nums_a) and j < len(nums_b):
        if nums_a[i] <= nums_b[j]:
            merged.append(nums_a[i]); i += 1
        else:
            merged.append(nums_b[j]); j += 1
    merged.extend(nums_a[i:])
    merged.extend(nums_b[j:])
    return merged
```

- LeetCode practice: Merge Two Sorted Lists, Merge Sorted Array, Interval List Intersections
- Show the interviewer: linear pass across both, stability if needed, edge cases when one list empties, O(n+m).

***

## 3. Sliding window

Fixed or variable window to maintain constraint.

```python
def max_sum_fixed_window(nums, window_size):
    window_sum = sum(nums[:window_size])
    best_sum = window_sum
    for end in range(window_size, len(nums)):
        window_sum += nums[end] - nums[end - window_size]
        best_sum = max(best_sum, window_sum)
    return best_sum
```

- LeetCode practice: Longest Substring Without Repeating Characters, Minimum Window Substring, Sliding Window Maximum, Permutation in String, Longest Repeating Character Replacement, Fruit Into Baskets
- Show the interviewer: when to expand/contract, invariants, O(n), using hashmap/counter for variable windows, careful index math.

***

## 4. Build a prefix sum

Range queries and subarray sums in O(1) after O(n) prep.

```python
def build_prefix_sums(nums):
    prefix = [0]
    for value in nums:
        prefix.append(prefix[-1] + value)
    return prefix  # range sum [l,r] = prefix[r+1] - prefix[l]
```

- LeetCode practice: Range Sum Query, Subarray Sum Equals K, Find Pivot Index, Maximum Size Subarray Sum Equals k
- Show the interviewer: off-by-one correctness, using dict for counts of prefix sums, memory vs recomputation trade-off.

***

## 5. Efficient string building

Use join list of chunks; avoid O(n^2) concatenation.

```python
def build_string(parts):
    return ''.join(parts)
```

- LeetCode practice: Multiply Strings, Add Strings, Decode String, Simplify Path
- Show the interviewer: time complexity of repeated concatenation vs join, streaming/stack approaches to generate parts.

***

## 6. Linked list: fast and slow pointer

Cycle detection, middle node, kth from end.

```python
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True
    return False
```

- LeetCode practice: Linked List Cycle, Linked List Cycle II, Middle of the Linked List, Remove Nth Node From End
- Show the interviewer: proof intuition (meeting point), O(1) space, null checks, variations (find cycle start).

***

## 7. Reversing a linked list

Iterative pointer rewiring.

```python
def reverse_list(head):
    previous = None
    current = head
    while current:
        nxt = current.next
        current.next = previous
        previous = current
        current = nxt
    return previous
```

- LeetCode practice: Reverse Linked List, Reverse Nodes in k-Group, Palindrome Linked List (half reverse)
- Show the interviewer: invariant (previous is reversed prefix), in-place O(1) space, handle empty/one-node.

***

## 8. Count subarrays that fit an exact criteria

Prefix-sum with hashmap for O(n) counting.

```python
from collections import defaultdict
def count_subarrays_with_sum(nums, target_sum):
    seen = defaultdict(int)
    seen[0] = 1
    running = 0
    count = 0
    for value in nums:
        running += value
        count += seen[running - target_sum]
        seen[running] += 1
    return count
```

- LeetCode practice: Subarray Sum Equals K, Binary Subarrays With Sum, Subarrays Divisible by K
- Show the interviewer: why hashmap works, zero-sum base, negative numbers allowed, difference from sliding window.

***

## 9. Monotonic increasing stack

Next/previous smaller/greater, ranges, histogram.

```python
def next_smaller_to_right(nums):
    stack = []  # store indices with increasing values
    answer = [-1] * len(nums)
    for idx, value in enumerate(nums):
        while stack and nums[stack[-1]] > value:
            answer[stack.pop()] = value
        stack.append(idx)
    return answer
```

- LeetCode practice: Daily Temperatures, Next Greater Element, Largest Rectangle in Histogram, Sum of Subarray Minimums, Removing K Digits
- Show the interviewer: what monotonic means, store indices vs values, amortized O(n), tie-handling for equal values.

***

## 10. Binary tree: DFS (recursive)

Inorder/preorder/postorder traversals.

```python
def inorder(node):
    if not node:
        return []
    return inorder(node.left) + [node.val] + inorder(node.right)
```

- LeetCode practice: Binary Tree Inorder Traversal, Validate BST (inorder sorted), Path Sum, Diameter of Binary Tree
- Show the interviewer: traversal orders, stack depth limits, when to build list vs yield/generate.

***

## 11. Binary tree: DFS (iterative)

Explicit stack simulating recursion.

```python
def inorder_iterative(root):
    result, stack = [], []
    current = root
    while stack or current:
        while current:
            stack.append(current)
            current = current.left
        current = stack.pop()
        result.append(current.val)
        current = current.right
    return result
```

- LeetCode practice: Iterative Traversals, BST Iterator, Kth Smallest in BST
- Show the interviewer: simulate call stack, push-left pattern, correctness for skewed trees.

***

## 12. Binary tree: BFS

Level order or shortest path in unweighted tree.

```python
from collections import deque
def level_order(root):
    if not root:
        return []
    levels, queue = [], deque([root])
    while queue:
        level_values = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level_values.append(node.val)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        levels.append(level_values)
    return levels
```

- LeetCode practice: Binary Tree Level Order Traversal, Average of Levels, Zigzag Level Order
- Show the interviewer: per-level loop using queue length, memory use in wide trees, when BFS > DFS.

***

## 13. Graph: DFS (recursive)

Explore connected components, topological checks (on DAGs with care).

```python
def dfs_recursive(adj, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    for neighbor in adj[start]:
        if neighbor not in visited:
            dfs_recursive(adj, neighbor, visited)
    return visited
```

- LeetCode practice: Number of Islands (grid DFS), Graph Traversal, Clone Graph (with map), Course Schedule (cycle detection)
- Show the interviewer: visited set, recursion depth risk, grid conversion to graph.

***

## 14. Graph: DFS (iterative)

Explicit stack + visited.

```python
def dfs_iterative(adj, start):
    stack, visited = [start], set()
    while stack:
        node = stack.pop()
        if node in visited:
            continue
        visited.add(node)
        for neighbor in adj[node]:
            if neighbor not in visited:
                stack.append(neighbor)
    return visited
```

- LeetCode practice: Graph Traversal, Number of Connected Components, Evaluate Division (path search)
- Show the interviewer: order not guaranteed, cycle-safe with visited, space trade-offs.

***

## 15. Graph: BFS

Shortest paths in unweighted graphs, layers.

```python
from collections import deque
def bfs_shortest_paths(adj, start):
    queue = deque([start])
    distance = {start: 0}
    while queue:
        node = queue.popleft()
        for neighbor in adj[node]:
            if neighbor not in distance:
                distance[neighbor] = distance[node] + 1
                queue.append(neighbor)
    return distance
```

- LeetCode practice: Word Ladder, Open the Lock, Perfect Squares (graph modeling), Rotting Oranges, 01 Matrix
- Show the interviewer: first-visit guarantees shortest path, predecessor map for path reconstruction.

***

## 16. Find top k elements with heap

Min-heap of size k or built-ins.

```python
import heapq
def top_k_largest(nums, k):
    return heapq.nlargest(k, nums)
```

- LeetCode practice: Top K Frequent Elements, Kth Largest Element in an Array, Sort Characters By Frequency, Find K Pairs with Smallest Sums
- Show the interviewer: min-heap of size k for streaming, O(n log k) vs sorting O(n log n), memory usage.

***

## 17. Binary Search

Classic template on sorted domain.

```python
def binary_search(sorted_nums, target):
    left, right = 0, len(sorted_nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if sorted_nums[mid] == target:
            return mid
        if sorted_nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

- LeetCode practice: Binary Search, Search Insert Position, Search in Rotated Sorted Array
- Show the interviewer: invariant (search space), mid computation, termination condition, avoiding infinite loops.

***

## 18. Binary search: duplicate elements, left-most insertion point

Lower bound pattern.

```python
def lower_bound(sorted_nums, target):
    left, right = 0, len(sorted_nums)
    while left < right:
        mid = (left + right) // 2
        if sorted_nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

- LeetCode practice: Find First and Last Position, Search Insert Position, Count of Range occurrences
- Show the interviewer: half-open interval [left,right), correctness with duplicates, post-checking bounds.

***

## 19. Binary search: duplicate elements, right-most insertion point

Upper bound pattern.

```python
def upper_bound(sorted_nums, target):
    left, right = 0, len(sorted_nums)
    while left < right:
        mid = (left + right) // 2
        if sorted_nums[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left
```

- LeetCode practice: Find First and Last Position, Count occurrences via upper-lower, Insert Position
- Show the interviewer: count = upper_bound - lower_bound, careful equality condition.

***

## 20. Binary search on answer (greedy check)

Search minimal feasible or maximal achievable value with a monotonic check.

```python
def min_capacity_to_ship(weights, days_limit):
    left, right = max(weights), sum(weights)
    while left < right:
        mid = (left + right) // 2
        days_used, current = 1, 0
        for w in weights:
            if current + w > mid:
                days_used += 1
                current = 0
            current += w
        if days_used > days_limit:
            left = mid + 1
        else:
            right = mid
    return left
```

- LeetCode practice: Capacity To Ship Packages, Koko Eating Bananas, Split Array Largest Sum, Minimize Max Distance to Gas Station
- Show the interviewer: monotonicity of predicate, write feasibility check first, search bounds, correctness argument.

***

## 21. Backtracking

Try choices, undo, prune with constraints.

```python
def permutations(nums):
    used = [False] * len(nums)
    result, path = [], []
    def dfs():
        if len(path) == len(nums):
            result.append(path[:]); return
        for i, value in enumerate(nums):
            if used[i]: continue
            used[i] = True; path.append(value)
            dfs()
            path.pop(); used[i] = False
    dfs()
    return result
```

- LeetCode practice: Permutations, Subsets, Combination Sum, N-Queens, Word Search, Palindrome Partitioning
- Show the interviewer: state (path, used), pruning early, ordering to avoid duplicates, complexity discussion.

***

## 22. Dynamic programming: top-down memoization

Recursive definition with caching.

```python
from functools import lru_cache
def climb_stairs(n):
    @lru_cache(None)
    def ways(step):
        if step <= 2:
            return step
        return ways(step - 1) + ways(step - 2)
    return ways(n)
```

- LeetCode practice: Climbing Stairs, House Robber, Coin Change, Edit Distance, Decode Ways, Unique Paths
- Show the interviewer: overlapping subproblems, base cases, memo key design, conversion to bottom-up if asked.

***

## 23. Build a trie

Prefix tree for string sets/prefix queries.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children: return False
            node = node.children[ch]
        return node.is_end
    def starts_with(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return False
            node = node.children[ch]
        return True
```

- LeetCode practice: Implement Trie, Word Search II, Add and Search Word (Wildcard), Longest Word in Dictionary, Replace Words
- Show the interviewer: time per op O(length), memory trade-offs, storing counts/ends, alternatives (hash set, sorting).

***

## 24. Dijkstra’s algorithm

Shortest paths with non-negative weights.

```python
import heapq
def dijkstra(adj, source):
    dist = {node: float('inf') for node in adj}
    dist[source] = 0
    heap = [(0, source)]
    while heap:
        d, node = heapq.heappop(heap)
        if d != dist[node]:
            continue
        for neighbor, weight in adj[node]:
            nd = d + weight
            if nd < dist[neighbor]:
                dist[neighbor] = nd
                heapq.heappush(heap, (nd, neighbor))
    return dist
```

- LeetCode practice: Network Delay Time, Path With Minimum Effort, Cheapest Flights Within K Stops (variation with constraints)
- Show the interviewer: non-negative edge weights requirement, lazy deletion via distance check, complexity O((V+E)logV).

***

## 25. Prim’s algorithm (Minimum Spanning Tree)

Grow MST from any start node with a cut of minimal edges.

```python
import heapq
def prim_mst(adj, start):
    visited = set([start])
    heap = []
    for v, w in adj[start]:
        heap.append((w, start, v))
    heapq.heapify(heap)
    total_weight = 0
    edges_used = 0
    while heap and edges_used < len(adj) - 1:
        w, u, v = heapq.heappop(heap)
        if v in visited:
            continue
        visited.add(v)
        total_weight += w
        edges_used += 1
        for nxt, nw in adj[v]:
            if nxt not in visited:
                heapq.heappush(heap, (nw, v, nxt))
    return total_weight
```

- LeetCode practice: Minimum Spanning Tree patterns (custom), Connecting Cities With Minimum Cost
- Show the interviewer: MST vs shortest paths, cut property, complexity O(E log V), graph representation.

***

## 26. Kruskal’s algorithm (Minimum Spanning Tree)

Greedy on edges with Union-Find.

```python
def kruskal_mst(node_count, edges):
    # edges: (weight, u, v)
    parent = list(range(node_count))
    rank = [0] * node_count
    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x
    def union(a, b):
        ra, rb = find(a), find(b)
        if ra == rb: return False
        if rank[ra] < rank[rb]:
            parent[ra] = rb
        elif rank[ra] > rank[rb]:
            parent[rb] = ra
        else:
            parent[rb] = ra
            rank[ra] += 1
        return True
    total = 0
    for w, u, v in sorted(edges):
        if union(u, v):
            total += w
    return total
```

- LeetCode practice: Kruskal patterns (custom), Minimum Cost to Connect All Points, Optimize Water Distribution in a Village
- Show the interviewer: Union-Find with path compression + union by rank, why sorting edges works, handling disconnected graphs.

***

# Quick Behavioral/Communication Tips (What to Showcase)

- Clarify constraints: input size, value ranges, sortedness, duplicates, negative numbers, streaming vs batch.
- State time/space upfront and justify choice of pattern.
- Maintain invariants: explain what the window/stack/heap/DS guarantees at each step.
- Edge cases: empty inputs, single element, all equal, negatives, duplicates, overflow, cycles.
- Testing aloud: small hand-run examples to verify off-by-one and boundary behavior.
- Complexity trade-offs: explain alternatives (e.g., sort vs heap; DFS vs BFS; Dijkstra vs BFS).
- Clean code: descriptive variable names, early exits, helper functions where appropriate.
- Incremental approach: brute force -> optimize; feasibility check for binary search on answer.

If helpful, I can convert this into a printable one-pager or tailor a focused practice plan by topic and difficulty.

