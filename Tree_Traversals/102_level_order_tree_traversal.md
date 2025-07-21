# 🌲 Leetcode 102: Binary Tree Level Order Traversal

---

## 🧩 Problem Statement

Given the `root` of a binary tree, return the *level order traversal* of its nodes' values (i.e., from left to right, level by level).

📘 **Example:**

```

Input:
3
/&#x20;
9  20
/ &#x20;
15   7

Output: \[\[3], \[9, 20], \[15, 7]]

````

---

## ✅ Expected in Interviews

Interviewers want to hear:
- Clear understanding of **BFS (Breadth-First Search)**
- Use of a **queue** to maintain level order
- How you differentiate nodes of each level (`level_size`)
- Handling of **edge cases** like `None` root
- Time/space complexity analysis

---

## 🔍 Edge Cases
| Case                     | Explanation                     |
|--------------------------|----------------------------------|
| Empty Tree (`root=None`) | Should return empty list `[]`   |
| Single Node              | Should return `[[val]]`         |
| Skewed Tree              | Still valid, output has 1 per level |

---

## 🧠 Intuition

We use a **queue** (FIFO) to explore the tree *level by level*.  
At each level, we:
- Count the number of nodes (`level_size`)
- Traverse those many nodes
- Add their children to the queue for the next level

---

## 🧪 Interview-Ready Code (BFS using Queue)

```python
from typing import Optional, List
from collections import deque

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # Result to hold the final level-wise list
        level_order_result = []
        
        # Edge case: empty tree
        if root is None:
            return level_order_result
        
        # Initialize queue with root node
        node_queue = deque([root])
        
        # Traverse level by level
        while node_queue:
            level_size = len(node_queue)  # Number of nodes at current level
            current_level = []
            
            for _ in range(level_size):
                current_node = node_queue.popleft()
                current_level.append(current_node.val)
                
                # Add left and right children to the queue if they exist
                if current_node.left:
                    node_queue.append(current_node.left)
                if current_node.right:
                    node_queue.append(current_node.right)
            
            # Add the collected values of the current level to result
            level_order_result.append(current_level)
        
        return level_order_result
````

---

## 📈 Complexity Analysis

| Metric   | Value                             |
| -------- | --------------------------------- |
| 🕒 Time  | `O(N)` — visit each node once     |
| 🧠 Space | `O(N)` — for the queue and result |

---

## 🔁 Alternate Approach: Recursive (DFS with Level Tracking)

Mentioning this shows depth even if you don’t implement it.

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        def dfs(node, level):
            if not node:
                return
            if len(result) == level:
                result.append([])  # Create a new level
            result[level].append(node.val)
            dfs(node.left, level + 1)
            dfs(node.right, level + 1)

        result = []
        dfs(root, 0)
        return result
```

🧠 *In this approach*, recursion helps us track depth and add values accordingly — but it's more natural to solve this with **BFS**.

---

## 🗣 What to Say in Interviews

> “I’ll use a queue to perform a level-order traversal (BFS).
> For each level, I’ll collect all node values into a list and push their children to the queue.
> I’ll keep track of the size of the current level to separate it from the next.”

> “Time is O(N), since we visit every node once, and space is O(N) due to the queue and result list.”

---

## 🧩 Follow-ups You May Get

* **Can you return values in reverse level order?** → Use `collections.deque` and appendleft()
* **Can you print the tree in zigzag fashion?** → Use a flag to reverse the level list before appending
* **Can you solve using recursion?** → Yes, with level-tracking DFS

---

## 🎯 Final Tips

✅ Use clear variable names like `node_queue`, `current_level`
✅ Practice both BFS (iterative) and DFS (recursive) versions
✅ Always handle `None` input
✅ Mention time and space trade-offs

---
