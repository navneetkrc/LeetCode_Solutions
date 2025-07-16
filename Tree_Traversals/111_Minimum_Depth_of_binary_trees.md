# 🌳 Minimum Depth of Binary Tree

### 🔍 Problem Statement ([LeetCode #111](https://leetcode.com/problems/minimum-depth-of-binary-tree/))

Given a binary tree, return *its minimum depth*.  
The **minimum depth** is the number of nodes along the **shortest path from the root node** down to the nearest **leaf node**.

> A **leaf** is a node with **no children**.

---

### 💡 Example
<img width="432" height="302" alt="image" src="https://github.com/user-attachments/assets/830a35de-8bd5-4ab0-8bec-af0edb81605c" />

```

Input: root = [3,9,20,null,null,15,7]
Output: 2
Example 2:

Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5

````

---

## ✅ Approach 1: Level Order Traversal (BFS)

### 📌 Why BFS?

- In BFS (level-order traversal), the **first leaf** node we encounter is **guaranteed to be at the minimum depth**.
- This is more efficient than DFS for this problem as it doesn't traverse unnecessary deeper branches.

---

### 🧠 Interview Expectations

- Understand the **difference between minimum depth and maximum depth**.
- Should **mention the strategy**: using BFS to return as soon as a leaf is found.
- Talk through **edge cases**: empty tree, root is a leaf.
- Clearly explain **why we stop at the first leaf**.

---

### ✅ Code (Well-commented & Interview-friendly)

```python
from collections import deque
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val: int = 0, left: Optional['TreeNode'] = None, right: Optional['TreeNode'] = None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # If the tree is empty, the minimum depth is 0
        if not root:
            return 0

        # Initialize a queue for BFS: stores tuples of (node, depth)
        bfs_queue = deque([(root, 1)])

        while bfs_queue:
            current_node, current_depth = bfs_queue.popleft()

            # If a leaf node is reached, return its depth (min depth)
            if not current_node.left and not current_node.right:
                return current_depth

            # Push left child to queue if it exists
            if current_node.left:
                bfs_queue.append((current_node.left, current_depth + 1))

            # Push right child to queue if it exists
            if current_node.right:
                bfs_queue.append((current_node.right, current_depth + 1))

        return 0  # Should never hit this due to return in loop
````

---

## 🔄 Pointer Illustration (for BFS Queue)

At start:

```
Queue = [(3,1)]
```

After 1st level:

```
Queue = [(9,2), (20,2)]
```

After popping 9 (leaf):

```
Leaf found → return 2
```

---

## ✅ Approach 2: DFS (Recursive)

### ⚠️ Interview Tip:

* DFS may explore full paths and hence is **less efficient** than BFS.
* But good to mention as an alternate approach.
* Be cautious when one subtree is `None`.

---

### 🔍 Code: DFS (Recursive)

```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # Base case: empty tree
        if not root:
            return 0

        # If left is None, recurse only on right
        if not root.left:
            return 1 + self.minDepth(root.right)

        # If right is None, recurse only on left
        if not root.right:
            return 1 + self.minDepth(root.left)

        # Both children exist: take the min of both depths
        return 1 + min(self.minDepth(root.left), self.minDepth(root.right))
```

---

### 🧪 Edge Cases to Mention in Interviews

| Case                             | Expected Output             |
| -------------------------------- | --------------------------- |
| Empty tree (`root = None`)       | `0`                         |
| Root only (no children)          | `1`                         |
| Skewed tree (only left or right) | Depth of longest path       |
| Balanced tree                    | Depth of shortest leaf path |

---

## 🧵 Time and Space Complexity

| Approach | Time Complexity | Space Complexity | Why?                                     |
| -------- | --------------- | ---------------- | ---------------------------------------- |
| BFS      | O(N)            | O(N)             | Visit each node once                     |
| DFS      | O(N)            | O(H)             | H = tree height (due to recursion stack) |

---

## 🧠 Summary for Interviews

* **Preferred Approach:** BFS with early leaf detection.
* **Key Insight:** First leaf encountered in BFS gives the minimum depth.
* **Edge Cases:** Empty tree, single node, skewed trees.
* **Alternate Approach:** DFS with careful handling of `None` children.
* Be ready to **draw trees and simulate levels** during interview!

---
