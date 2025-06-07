
---

# 🧗‍♂️ Leetcode 1306 - Jump Game III

## 📘 Problem Statement

You are given an array of non-negative integers `arr` and an integer `start` index.
You can jump from index `i` to:

* `i + arr[i]` (right)
* `i - arr[i]` (left)

Return `True` if you can **reach any index with value `0`**, otherwise return `False`.

**Constraints**:

* `1 <= arr.length <= 5 * 10^4`
* `0 <= arr[i] < arr.length`
* `0 <= start < arr.length`

---

## 🔍 Example

```python
Input: arr = [4,2,3,0,3,1,2], start = 5
Output: True
Explanation:
5 ➝ 4 ➝ 1 ➝ 3 (which is 0)
```

---

## 🧠 What Interviewers Expect

Interviewers assess:

* ✅ Your understanding of search (DFS/BFS)
* ✅ How you avoid infinite loops (visited tracking)
* ✅ Code clarity and maintainability
* ✅ Optimization of recursive calls
* ✅ Use of space and time efficiently

---

## 🌀 Approach 1: Recursive Backtracking (DFS)

### 💡 Intuition

Use recursion to explore both left and right jumps. Keep track of visited indices to avoid cycles. If we land on a `0`, return success.

### ✅ Pros:

* Simple, intuitive
* Good for recursive problem-solving practice

### ❌ Cons:

* Can cause **stack overflow** on large inputs
* Inefficient without optimization

```python
from typing import List

class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        n = len(arr)
        visited = [False] * n  # to track visited indices

        def dfs(index: int) -> bool:
            # Base cases
            if index < 0 or index >= n or visited[index]:
                return False

            if arr[index] == 0:
                return True

            visited[index] = True  # mark as visited

            # Try both directions
            return dfs(index + arr[index]) or dfs(index - arr[index])

        return dfs(start)
```

---

## 🚀 Approach 2: BFS (Queue)

### 💡 Intuition

Use a queue to **level-wise** explore all reachable indices.
Mark visited nodes by converting values to negative.

### ✅ Pros:

* No recursion depth issues
* Easy to control visited
* Efficient and clean

### ❌ Cons:

* Mutates input array (can be avoided)

```python
from collections import deque
from typing import List

class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        n = len(arr)
        queue = deque([start])

        while queue:
            current = queue.popleft()

            # Success condition
            if arr[current] == 0:
                return True

            # Skip already visited
            if arr[current] < 0:
                continue

            # Try both directions
            jump_distance = arr[current]

            if current + jump_distance < n:
                queue.append(current + jump_distance)

            if current - jump_distance >= 0:
                queue.append(current - jump_distance)

            # Mark visited
            arr[current] = -arr[current]

        return False
```

---

## 🌲 Approach 3: Binary Tree Style Recursion (With Memoization)

### 💡 Intuition

Imagine the problem as a **binary tree**:

* Each node represents an index
* Left child → index - arr\[index]
* Right child → index + arr\[index]

Use a **set to memoize visited nodes** and avoid reprocessing them.

```python
from typing import List

class TreeNodeDFS:
    def canReach(self, arr: List[int], start: int) -> bool:
        visited = set()

        def build_tree(index: int) -> bool:
            # Boundary or revisit check
            if index < 0 or index >= len(arr) or index in visited:
                return False

            if arr[index] == 0:
                return True

            visited.add(index)

            left = index - arr[index]
            right = index + arr[index]

            # Recur for children (left and right jumps)
            return build_tree(left) or build_tree(right)

        return build_tree(start)
```

---

## 🌟 Visual Representation

```
Start: index 5, arr[5] = 1

        5
      ↙   ↘
     4     6
    ↙       ↘
   1         4  (repeat)
  ↙ ↘
 3   3   ← Found arr[3] == 0 ✅
```

---

## 🆚 Summary of All Approaches

| Approach       | Space | Time | Stack-safe | Notes                                  |
| -------------- | ----- | ---- | ---------- | -------------------------------------- |
| DFS Recursion  | O(n)  | O(n) | ❌ No       | Needs visited\[] to avoid TLE/loops    |
| BFS (Queue)    | O(n)  | O(n) | ✅ Yes      | Preferred in interviews for large data |
| Tree-style DFS | O(n)  | O(n) | ❌ No       | Same as DFS but easier to visualize    |

---

## 💬 Final Notes

* Always track visited nodes to avoid infinite loops.
* Avoid mutating input unless clearly acceptable.
* Prefer **BFS** in interviews for robustness.
* Explain **intuition**, **boundary checks**, and **visited logic** while discussing.


---
claude4 Version
---
gemini pro june 05
---
Of course! This is an excellent problem to discuss in an interview because it's a classic graph traversal problem disguised as an array problem. Let's break down the concepts, refine the code, and structure it for clarity.

***

## 🎯 Jump Game III: A Deep Dive into Graph Traversal

The core of this problem is not about arrays, but about **reachability in a graph**. Recognizing this is the most crucial first step in an interview.

*   **Nodes (Vertices):** Each index `i` of the array `arr` is a node in our graph.
*   **Edges (Connections):** From any node `i`, there are directed edges to two other nodes: `i + arr[i]` and `i - arr[i]` (if they are within the array's bounds).
*   **Goal:** The problem asks: "Is there a path from the `start` node to *any* node `j` where the value `arr[j]` is 0?"

Since we are looking for *any* path, this is a perfect use case for standard graph traversal algorithms like **Depth-First Search (DFS)** or **Breadth-First Search (BFS)**.

A critical challenge in graph traversal is handling **cycles**. If we can jump from index `i` to `j` and back to `i`, we could get stuck in an infinite loop. To prevent this, we must keep track of the nodes (indices) we have already visited.

---

### Approach 1: Depth-First Search (DFS) using Recursion

DFS explores as far as possible down one path before backtracking. It's often implemented elegantly with recursion, which uses the call stack to manage the "backtracking" process.

#### How it Works
1.  Start a traversal from the `start` index.
2.  Keep a `visited` set to store indices we've already explored. This is essential to prevent infinite loops.
3.  From the current index, check for the base cases:
    *   **Out of Bounds:** If the index is invalid (`< 0` or `>= len(arr)`), this path is a dead end. Return `False`.
    *   **Already Visited:** If we've been to this index before, we are in a cycle or a previously explored path. Return `False` to prune this branch.
    *   **Target Found:** If `arr[current_index]` is 0, we've succeeded! Return `True`.
4.  If none of the base cases are met, we are at a valid, new index.
    *   **Mark as Visited:** Add the current index to our `visited` set.
    *   **Explore Neighbors:** Recursively call the function for both possible jumps: `current_index + arr[current_index]` and `current_index - arr[current_index]`.
    *   **Propagate Result:** If *either* of the recursive calls returns `True`, it means a path to a zero was found, so we return `True`.

#### Refined Python Code (DFS)

```python
from typing import List, Set

class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        """
        Determines if an index with value 0 can be reached using DFS.
        
        This problem can be modeled as a graph where array indices are nodes
        and jumps are edges. We use DFS to explore this graph.
        """
        # A set to keep track of visited indices to avoid cycles and redundant work.
        visited = set()
        
        # Call the recursive helper function to start the search.
        return self._dfs(arr, start, visited)

    def _dfs(self, arr: List[int], current_index: int, visited: Set[int]) -> bool:
        # --- Base Cases for Recursion ---

        # 1. Check if the index is out of the array bounds.
        if not (0 <= current_index < len(arr)):
            return False

        # 2. Check if the index has already been visited. This prevents infinite loops.
        if current_index in visited:
            return False

        # 3. Check if we have reached a target index (value is 0).
        if arr[current_index] == 0:
            return True

        # --- Recursive Step ---

        # Mark the current index as visited before exploring its neighbors.
        visited.add(current_index)

        # Explore the two possible jump directions.
        jump_value = arr[current_index]
        can_reach_from_forward_jump = self._dfs(arr, current_index + jump_value, visited)
        can_reach_from_backward_jump = self._dfs(arr, current_index - jump_value, visited)

        # If either path leads to a solution, we can reach a zero.
        return can_reach_from_forward_jump or can_reach_from_backward_jump

```

#### Complexity Analysis
*   **Time Complexity: O(N)**, where N is the number of elements in `arr`. In the worst case, we visit each index exactly once.
*   **Space Complexity: O(N)**. This is due to two factors:
    1.  The `visited` set can store up to N indices.
    2.  The recursion depth can go up to N in the worst case (e.g., a path that visits every node in a chain).

---

### Approach 2: Breadth-First Search (BFS) using a Queue

BFS explores the graph layer by layer. It's guaranteed to find the *shortest* path in terms of the number of jumps, although that's not a requirement for this problem. It uses a queue to manage the order of nodes to visit.

#### How it Works
1.  Initialize a `queue` and add the `start` index to it.
2.  Initialize a `visited` set and add the `start` index to it.
3.  Loop as long as the `queue` is not empty:
    *   Dequeue the next `current_index` to visit.
    *   Check if `arr[current_index]` is 0. If yes, return `True`.
    *   Calculate the two possible next jumps.
    *   For each valid jump, if the destination index is within bounds and has **not** been visited, add it to both the `queue` and the `visited` set.
4.  If the loop finishes, it means we've explored all reachable indices from `start` and none had a value of 0. Return `False`.

#### Refined Python Code (BFS)

```python
from collections import deque
from typing import List, Set

class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        """
        Determines if an index with value 0 can be reached using BFS.
        
        This approach explores the graph layer by layer, starting from the `start` index.
        A queue is used to manage the nodes to visit.
        """
        n = len(arr)
        queue = deque([start])  # Initialize queue with the starting position
        visited = {start}       # Use a set for O(1) average time complexity for lookups

        while queue:
            current_index = queue.popleft()

            # Check if we've reached a target index
            if arr[current_index] == 0:
                return True

            jump_value = arr[current_index]

            # --- Explore Neighbors ---
            
            # 1. Jump forward
            next_index_forward = current_index + jump_value
            if next_index_forward < n and next_index_forward not in visited:
                visited.add(next_index_forward)
                queue.append(next_index_forward)

            # 2. Jump backward
            next_index_backward = current_index - jump_value
            if next_index_backward >= 0 and next_index_backward not in visited:
                visited.add(next_index_backward)
                queue.append(next_index_backward)
        
        # If the queue becomes empty, we've explored all reachable nodes
        # without finding a zero.
        return False
```

#### Space Optimization (Interview Pro-Tip)

If modifying the input array is allowed, you can achieve **O(1) extra space** for the `visited` tracker. Since all `arr[i]` are non-negative, we can mark an index as visited by making its value negative.

```python
from collections import deque
from typing import List

class Solution:
    def canReach_OptimizedSpace(self, arr: List[int], start: int) -> bool:
        # This version modifies the input array to track visited nodes, saving space.
        n = len(arr)
        queue = deque([start])

        while queue:
            current_index = queue.popleft()

            # If the value is 0, we found it.
            if arr[current_index] == 0:
                return True
            
            jump_value = arr[current_index]
            
            # Mark the current index as visited by negating its value.
            # This works because all original values are non-negative.
            arr[current_index] = -jump_value 

            # Explore forward
            next_index_forward = current_index + jump_value
            if next_index_forward < n and arr[next_index_forward] >= 0: # Check if not visited
                queue.append(next_index_forward)

            # Explore backward
            next_index_backward = current_index - jump_value
            if next_index_backward >= 0 and arr[next_index_backward] >= 0: # Check if not visited
                queue.append(next_index_backward)

        return False
```
> **Interview Note:** Always ask the interviewer if modifying the input is permissible before presenting this optimized solution.

#### Complexity Analysis
*   **Time Complexity: O(N)**. Each index is enqueued and processed at most once.
*   **Space Complexity:**
    *   Standard BFS: **O(N)** for the `queue` and `visited` set. In the worst case, all nodes might be added to the queue (e.g., a star graph).
    *   Optimized BFS: **O(N)** for the `queue`, but **O(1)** extra space for the `visited` tracker.

---

### The "Tree Construction" Mental Model

You asked for a tree construction approach. This is an excellent intuition! The exploration process of both DFS and BFS can be visualized as building a **traversal tree**.

*   **Root:** The `start` index.
*   **Children:** The indices you can jump to (`i + arr[i]` and `i - arr[i]`).

However, it's not a true tree because a node can be a child of multiple parents. For example, index `i` can be reached from `i - arr[i]` and `i + arr[i]`. This is why it's technically a **graph**.

**Optimizing for "Repeat of Child"**

This is the key insight! A "repeat of a child" means we are trying to visit a node that has already been explored. **This is precisely what the `visited` set solves.**

When we check `if next_index in visited`, we are "pruning" the exploration. We are saying, "We have already explored this entire sub-graph, so there's no need to do it again." This prevents redundant computations and, crucially, avoids getting trapped in infinite cycles.

So, while you don't need to build an explicit `TreeNode` class, thinking about the traversal as building a tree and pruning already-seen branches is the perfect mental model for understanding why a `visited` set is necessary and effective. The DFS and BFS implementations above are the correct and efficient ways to realize this "tree/graph exploration."

### Summary for Interviews

| Feature             | DFS (Recursive)                                                                      | BFS (Iterative with Queue)                                                                  |
| ------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Algorithm**       | Explores depth-first. "Go deep, then backtrack."                                       | Explores layer-by-layer. "Explore neighbors, then their neighbors."                         |
| **Data Structure**  | Implicitly uses the system's call stack.                                               | Explicitly uses a `Queue`.                                                                  |
| **Path Finding**    | Finds *a* path, not necessarily the shortest.                                          | Finds the *shortest* path (in terms of number of jumps).                                    |
| **Space Complexity**| O(N) for `visited` set and recursion stack. Can cause a `StackOverflowError` for deep paths. | O(N) for `visited` set and queue. Safer for deep/long graphs. Can be optimized to O(1) extra space. |
| **When to Choose**  | Simple to write recursively. Good for general path finding if stack depth isn't a concern. | Generally safer against stack overflow. The natural choice for finding the shortest path. |
---

Would you like a **graphical diagram (image)** to visualize the recursion tree or BFS traversal path as well?
