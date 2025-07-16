# 🌲 Maximum Depth of Binary Tree

### 🔍 Problem Statement ([LeetCode #104](https://leetcode.com/problems/maximum-depth-of-binary-tree/))

Given the `root` of a binary tree, return **its maximum depth**.

> The **maximum depth** is the number of nodes along the **longest path** from the root node down to the farthest **leaf node**.

---

### 💡 Example


```

Input: root = [3,9,20,null,null,15,7]
Output: 3

Explanation:
Longest path = 3 → 20 → 15 (or 7), so max depth = 3.


Example 2:

Input: root = [1,null,2]
Output: 2


````

---

## ✅ Approach 1: DFS (Recursive)

### 📌 Why Recursive DFS?

- Depth is inherently a recursive property: the depth of a node is `1 + max(depth of left, depth of right)`.
- This approach mirrors the structure of the tree.

---

### 🧠 Interview Expectations

- Be clear about **what depth means** (path from root to the farthest leaf).
- Mention **DFS traversal**, and how recursion models tree traversal naturally.
- Talk through **base cases**, and the recursive relation.
- If asked for iterative DFS or BFS versions, be prepared to discuss them.

---

### ✅ Code (Well-commented & Interview-friendly)

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val: int = 0, left: Optional['TreeNode'] = None, right: Optional['TreeNode'] = None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # Base Case: if the node is None, depth is 0
        if not root:
            return 0

        # Recursively find depth of left and right subtrees
        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)

        # Depth of current node = 1 + max of left and right subtree depths
        return 1 + max(left_depth, right_depth)
````

---

### 🧠 Visualization: How DFS Builds Up

```
          3
         / \
        9  20
           / \
          15  7

DFS Recursion:
maxDepth(3) = 1 + max(maxDepth(9), maxDepth(20))
              = 1 + max(1, 1 + max(1, 1)) = 3
```

---

## ✅ Approach 2: BFS (Level Order Traversal)

### 📌 Why BFS?

* Process level by level.
* The number of levels = maximum depth.
* Use a queue to track levels.

---

### 🔍 Code: BFS Level-Order

```python
from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        queue = deque([root])
        depth = 0

        while queue:
            level_size = len(queue)
            for _ in range(level_size):
                node = queue.popleft()

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            # After finishing current level
            depth += 1

        return depth
```

---

## 🧪 Edge Cases to Mention in Interviews

| Case                            | Expected Output           |
| ------------------------------- | ------------------------- |
| Empty tree (`root = None`)      | `0`                       |
| Root only (no children)         | `1`                       |
| Skewed tree (all left or right) | Equal to number of nodes  |
| Balanced tree                   | Longest root-to-leaf path |

---

## 🧵 Time and Space Complexity

| Approach | Time Complexity | Space Complexity | Notes                                |
| -------- | --------------- | ---------------- | ------------------------------------ |
| DFS      | O(N)            | O(H)             | H = height of tree (recursion stack) |
| BFS      | O(N)            | O(W)             | W = max width of tree (queue size)   |

---

## 🧠 Summary for Interviews

* Use DFS recursion for clean and intuitive solution.
* Use BFS if interviewer hints toward level-order traversal.
* Clearly explain difference between **min depth** and **max depth**.
* Know both recursive and iterative approaches.
* Show confidence with base cases and recursive breakdown.

---

### 🔁 Difference from Min Depth?

| Concept     | Min Depth                    | Max Depth                            |
| ----------- | ---------------------------- | ------------------------------------ |
| Stops Early | ✅ (first leaf found via BFS) | ❌ (must explore entire tree)         |
| Use Case    | Optimize search              | Measure structure                    |
| Preferred   | BFS or DFS with condition    | DFS (recursive) or BFS (level count) |

---
