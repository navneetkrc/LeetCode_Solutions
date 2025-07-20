# 🌲 Infographic Definition: Tree Class (Binary Tree & Beyond)

---

## 🔧 Basic Tree Node Structure

In most interview settings (like LeetCode, FAANG interviews), binary trees are structured as follows:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val            # 🌟 Value of the node
        self.left = left          # 👈 Left child (TreeNode)
        self.right = right        # 👉 Right child (TreeNode)
```

---

## 🌳 Visual Example

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

```python
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
root.right.right = TreeNode(6)
```

---

## 📚 Popular Interview Questions Using This Class

| Problem                               | Type                   | Key Technique          |
| ------------------------------------- | ---------------------- | ---------------------- |
| Invert Binary Tree                    | DFS/BFS                | Swap children          |
| Max Depth of Binary Tree              | DFS (post-order)       | Recursive height       |
| Diameter of Binary Tree               | DFS                    | Pass height & diameter |
| Path Sum                              | DFS                    | Track cumulative sum   |
| Symmetric Tree                        | Mirror traversal       | Recursive check        |
| Level Order Traversal                 | BFS                    | Use queue              |
| Serialize and Deserialize Binary Tree | Preorder + Null marker | Recursion / Iterative  |

---

## 👪 Extended TreeNode with Parent Pointer

In some problems, especially those requiring **ancestor**, **upward traversal**, or **graph-like traversal**, it's useful to have:

```python
class TreeNodeWithParent:
    def __init__(self, val=0, left=None, right=None, parent=None):
        self.val = val
        self.left = left
        self.right = right
        self.parent = parent  # 🔼 Reference to the parent node
```

---

## 📘 Parent-Pointer: When Is It Useful?

### 1. 🔍 **Lowest Common Ancestor (LCA)** — without recursion

```python
def findLCA(node1, node2):
    ancestors = set()
    while node1:
        ancestors.add(node1)
        node1 = node1.parent
    while node2:
        if node2 in ancestors:
            return node2
        node2 = node2.parent
```

📌 This avoids recursion or needing to traverse from root.

---

### 2. 🔁 **Iterative Post-order / Reverse Path Traversal**

Walk from a node **up to the root** (e.g., print path, compute depth):

```python
def getDepth(node):
    depth = 0
    while node:
        depth += 1
        node = node.parent
    return depth
```

---

## 🧠 Summary: TreeNode vs TreeNodeWithParent

| Feature                  | TreeNode | TreeNodeWithParent      |
| ------------------------ | -------- | ----------------------- |
| Use in 95% problems      | ✅ Yes    | ⛔ Not required          |
| Helpful for ancestor ops | ❌ No     | ✅ Yes                   |
| Cyclic graph risks       | ❌ No     | ⚠️ Yes (careful in DFS) |

---

## 🧩 Design Tip for Interviews

> Start with the basic class.
> Extend with `.parent` only **if** the problem explicitly allows or requires upward traversal.

---


# 🌳 Complete Binary Tree – Infographic Definition

https://medium.com/data-science/5-types-of-binary-tree-with-cool-illustrations-9b335c430254

---
<img width="1001" height="601" alt="image" src="https://github.com/user-attachments/assets/3af3673e-e156-4247-abe7-5146ed06e59d" />

---
## ✅ **Definition**

A **Complete Binary Tree** is a binary tree in which:

* All levels are **completely filled**, **except possibly the last level**.
* The last level has **all nodes as far left as possible**.

📘 **Visual Rule**:
📏 Fill from **left to right**, top to bottom – no "gaps" before the last node.

---
A **complete binary tree** is a special type of binary tree where all levels of the tree are completely filled, except possibly the last level. If the last level is not completely filled, its nodes must be filled from left to right without any gaps.

Key characteristics of a complete binary tree:
*   **Level Filling** Every level, apart from the last one, must be full. This means a level at depth `d` must contain exactly $$2^d$$ nodes.
*   **Last Level Alignment** The nodes on the final level must be as far left as possible. A node is not permitted to have a right child unless it also has a left child.


### How it Differs from Other Binary Trees

It is important to distinguish a complete binary tree from other related types:

| Tree Type | Definition |
| :--- | :--- |
| **Complete Binary Tree** | All levels are filled, except possibly the last, which is filled left-to-right. |
| **Full Binary Tree** | Every node has either zero or two children. The nodes in a full tree do not need to be organized in a specific way beyond this rule. |
| **Perfect Binary Tree** | A tree in which all interior nodes have two children and all leaf nodes are at the same depth. A perfect binary tree is always complete, but a complete tree is not always perfect. |

Complete binary trees are particularly efficient for implementing heap data structures and priority queues because their structure allows for optimized storage and predictable node placement.

---

## 📊 **Illustration**

```
      1
     / \
    2   3
   / \  /
  4  5 6    ✅ Complete
```

```
      1
     / \
    2   3
   /     \
  4       5    ❌ Not Complete (missing 2nd level right before filling last)
```

---

## 📐 **Properties**

| Feature                | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| ✅ All levels filled    | Except last level                                      |
| ✅ Left-packed          | Nodes must appear **left to right** at the last level  |
| ❌ Not necessarily full | Children may be missing on **right** at the last level |
| ⏱ Height               | `floor(log₂(n))` where `n` = total nodes               |

---

## 🧠 **Why It Matters in Interviews**

* Used in **Heap** data structures (Min-Heap / Max-Heap).
* **Efficient indexing** in array representation (e.g., for heaps).
* Helps optimize **tree balancing** logic.

---

## 🛠 **Checking for Completeness in Code**

```python
from collections import deque

def isCompleteTree(root):
    q = deque([root])
    end = False
    
    while q:
        node = q.popleft()
        if not node:
            end = True
        else:
            if end:
                return False  # Found node after a null → Not complete
            q.append(node.left)
            q.append(node.right)
    
    return True
```

🧩 **Time**: O(N)
🧠 **Space**: O(N) (queue for level-order traversal)

---

## 🔖 Summary

> A Complete Binary Tree is a **left-filled**, level-ordered tree where no node can appear after a gap.

🎯 **Key Use Cases**: Heap implementation, balanced tree scenarios, and tree-based array optimizations.

---
Absolutely! Here's a **visually rich markdown-based cheat sheet** comparing **DFS**, **BFS**, and **Parent Traversal** in tree problems, highlighting:

* 🔧 Structure
* 🚀 Use Cases
* 🧠 Core Logic
* 🛠️ Code Snippets
* 🧩 Interview Examples

---

# 🧠 Visual Cheatsheet: DFS vs BFS vs Parent Traversal in Trees

---

## 🌿 1. Depth-First Search (DFS)

### 🚀 **Explores one path fully before backtracking**

### 🔧 Traversal Types:

* **Pre-order**: root → left → right
* **In-order**: left → root → right
* **Post-order**: left → right → root

### 🧠 When to Use:

* Computing **height**, **diameter**, **sum**
* **Path-based** problems (e.g., root-to-leaf sum)
* Recursive or stack-based logic

### 📘 Sample Code (Post-order DFS):

```python
def dfs(node):
    if not node:
        return 0
    left = dfs(node.left)
    right = dfs(node.right)
    return 1 + max(left, right)
```

### 📌 Used In:

* Max Depth / Min Depth
* Path Sum
* Diameter of Tree
* Invert / Mirror Tree
* Tree Traversals (Recursive)

---

## 🍃 2. Breadth-First Search (BFS)

### 🚀 **Explores level-by-level, using a queue**

### 🧠 When to Use:

* Problems that involve **level order**, **shortest path**, or **distance from root**
* Questions involving **layers** or **symmetric structure**

### 🔧 BFS Queue Logic:

```python
from collections import deque

def bfs(root):
    if not root:
        return
    queue = deque([root])
    while queue:
        node = queue.popleft()
        process(node)
        if node.left: queue.append(node.left)
        if node.right: queue.append(node.right)
```

### 📌 Used In:

* Level Order Traversal
* Symmetric Tree
* Binary Tree Right View
* Minimum Depth of Tree
* Serialize/Deserialize Tree

---

## 🌳 3. Parent-Pointer Traversal

### 🚀 **Moves upward from node to root via `.parent`**

### 🧠 When to Use:

* **LCA** when root isn’t available
* **Trace path up**, e.g., printing ancestry
* **Bidirectional search** on tree/graph

### 🔧 Logic:

```python
def getDepth(node):
    depth = 0
    while node:
        node = node.parent
        depth += 1
    return depth
```

### 📌 Used In:

* Lowest Common Ancestor (without root)
* Print path to root
* Upward traversal optimizations
* Path reversal

---

## 📊 Summary Table

| Feature        | DFS                      | BFS                       | Parent Traversal            |
| -------------- | ------------------------ | ------------------------- | --------------------------- |
| **Approach**   | Stack / Recursion        | Queue                     | Upward via `.parent`        |
| **Space**      | O(h)                     | O(n)                      | O(h)                        |
| **Use for**    | Depth, Path, Subtree Ops | Level, Distance, Layers   | Ancestors, LCA without root |
| **Code Style** | Recursive or stack       | Queue (iterative)         | While loop upward           |
| **Common in**  | DFS-based tree problems  | Level-order type problems | Tree with parent pointer    |

---

## 🎯 Decision Tree

```text
|– Do you need level info? → BFS
|– Do you need to reach deepest node or compute something recursively? → DFS
|– Do you need to trace upward or access ancestors without root? → Parent Traversal
```

---
Absolutely! Here's a **🎯 Full Cheatsheet for TreeNode-based Patterns** — optimized for **coding interviews**, **DSA prep**, and **visual clarity**.

---

# 🌳 TreeNode-Based Interview Cheatsheet

> Covers: Traversals • Recursion • Ancestors • Path Problems • Modifications • DFS/BFS • Parent Pointers

---

## 🧱 Basic TreeNode Structure

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None, parent=None):
        self.val = val
        self.left = left
        self.right = right
        self.parent = parent  # optional for upward traversal
```

---

## 🔁 TRAVERSALS (DFS / BFS)

### 🌿 Depth-First Search (DFS)

| Type      | Order               | Use Case                        |
| --------- | ------------------- | ------------------------------- |
| Preorder  | root → left → right | Copy tree, serialize            |
| Inorder   | left → root → right | Sorted data (BST), validate BST |
| Postorder | left → right → root | Delete tree, subtree-based ops  |

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val)
        inorder(root.right)
```

### 🍃 Breadth-First Search (BFS)

```python
from collections import deque

def levelOrder(root):
    q = deque([root])
    while q:
        node = q.popleft()
        print(node.val)
        if node.left: q.append(node.left)
        if node.right: q.append(node.right)
```

---

## 🧠 CORE PATTERNS

### ✅ 1. **Max Depth / Min Depth**

```python
# Max Depth
def maxDepth(root):
    if not root: return 0
    return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

---

### ✅ 2. **Check if Tree is Balanced**

```python
def isBalanced(root):
    def height(node):
        if not node: return 0
        lh, rh = height(node.left), height(node.right)
        if lh == -1 or rh == -1 or abs(lh - rh) > 1: return -1
        return 1 + max(lh, rh)
    return height(root) != -1
```

---

### ✅ 3. **Symmetric Tree**

```python
def isSymmetric(root):
    def isMirror(t1, t2):
        if not t1 and not t2: return True
        if not t1 or not t2: return False
        return t1.val == t2.val and isMirror(t1.left, t2.right) and isMirror(t1.right, t2.left)
    return isMirror(root, root)
```

---

### ✅ 4. **Path Sum (Root to Leaf)**

```python
def hasPathSum(root, target):
    if not root: return False
    if not root.left and not root.right:
        return target == root.val
    return hasPathSum(root.left, target - root.val) or hasPathSum(root.right, target - root.val)
```

---

### ✅ 5. **All Paths from Root to Leaf**

```python
def binaryTreePaths(root):
    res = []
    def dfs(node, path):
        if not node: return
        path += str(node.val)
        if not node.left and not node.right:
            res.append(path)
        else:
            dfs(node.left, path + '->')
            dfs(node.right, path + '->')
    dfs(root, '')
    return res
```

---

## 👨‍👧‍👦 ANCESTORS & PARENT POINTERS

### ✅ 6. **Lowest Common Ancestor (with root access)**

```python
def lca(root, p, q):
    if not root or root == p or root == q: return root
    left, right = lca(root.left, p, q), lca(root.right, p, q)
    return root if left and right else left or right
```

---

### ✅ 7. **LCA using Parent Pointer**

```python
def lcaWithParent(p, q):
    seen = set()
    while p or q:
        if p:
            if p in seen: return p
            seen.add(p)
            p = p.parent
        if q:
            if q in seen: return q
            seen.add(q)
            q = q.parent
```

---

### ✅ 8. **Find Distance Between Two Nodes**

```python
def findDistance(root, p, q):
    def lca(node, p, q):
        if not node or node == p or node == q: return node
        left, right = lca(node.left, p, q), lca(node.right, p, q)
        return node if left and right else left or right

    def depth(node, target, d):
        if not node: return -1
        if node == target: return d
        return max(depth(node.left, target, d + 1), depth(node.right, target, d + 1))

    ancestor = lca(root, p, q)
    return depth(ancestor, p, 0) + depth(ancestor, q, 0)
```

---

## 🔄 TREE MODIFICATIONS

### ✅ 9. **Invert a Tree (Mirror Image)**

```python
def invertTree(root):
    if root:
        root.left, root.right = invertTree(root.right), invertTree(root.left)
    return root
```

---

### ✅ 10. **Serialize / Deserialize**

```python
# Preorder-based serialization
def serialize(root):
    if not root: return 'N,'
    return str(root.val) + ',' + serialize(root.left) + serialize(root.right)

def deserialize(data):
    vals = iter(data.split(','))
    def build():
        val = next(vals)
        if val == 'N': return None
        node = TreeNode(int(val))
        node.left = build()
        node.right = build()
        return node
    return build()
```

---

## 🎯 Decision Map

```txt
       ┌────────────────────────┐
       │ Level-based Problem?   │──► Use BFS
       └────────────────────────┘
                 │
                 ▼
       ┌────────────────────────┐
       │ Subtree / Depth-based? │──► Use DFS
       └────────────────────────┘
                 │
                 ▼
       ┌────────────────────────────┐
       │ Ancestors / Parent Access? │──► Use Parent Pointers
       └────────────────────────────┘
```

---

## 🛠 Utility Templates

```python
# General DFS Template
def dfs(node):
    if not node: return
    dfs(node.left)
    dfs(node.right)

# BFS with Level Tracking
from collections import deque
def bfs(root):
    q = deque([root])
    while q:
        for _ in range(len(q)):
            node = q.popleft()
            # do something
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
```

---

## 🧩 Top 10 Practice Questions (LeetCode)

| Problem                       | Type       |
| ----------------------------- | ---------- |
| 104. Max Depth                | DFS        |
| 543. Diameter of Binary Tree  | DFS        |
| 226. Invert Binary Tree       | DFS        |
| 101. Symmetric Tree           | BFS        |
| 112. Path Sum                 | DFS        |
| 617. Merge Two Binary Trees   | DFS        |
| 236. Lowest Common Ancestor   | DFS/Parent |
| 102. Binary Tree Level Order  | BFS        |
| 124. Max Path Sum             | Postorder  |
| 129. Sum Root to Leaf Numbers | DFS        |

---

