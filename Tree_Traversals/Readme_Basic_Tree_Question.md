

# 🌲 **Infographic Notes: Interview Tree Coding Guide**

---
[Binary Trees & Binary Search Trees – DSA Course in Python Lecture 8 (Greg Hogg) (covers tree structures, traversals, and BST search)](https://www.youtube.com/watch?v=EPwWrs8OtfI&utm_source=chatgpt.com)

---
## 🧠 **1. Tree Types & Representation**

* **Binary Tree**: Each node has up to two children (left/right).
* **BST (Binary Search Tree)**: Enforces left < node < right property.
* Stored via node references or as 1-index arrays ([YouTube][1]).

---

## 🔁 **2. Core Recursive Strategy**

1. **Base Case**: Handle `None` (e.g., return default or `False`).
2. **Recurse Left**: Call on `root.left`.
3. **Recurse Right**: Call on `root.right`.
4. **Combine & Return**: Aggregate child results.

This pattern underlies most tree solutions ([AlgoMap][2]).

---

## 🔍 **3. Key Traversal Methods**

* **Pre‑order (DFS)**: `node → left → right`
* **In‑order (DFS)**: `left → node → right`
* **Post‑order (DFS)**: `left → right → node`
* **Level‑order (BFS)**: Across levels using a queue ([AlgoMap][2]).

```python
# Recursive pre-order (DFS)
def preOrder(node):
    if not node:
        return
    print(node.val)
    preOrder(node.left)
    preOrder(node.right)
```

```python
# Level-order (BFS)
from collections import deque
def levelOrder(root):
    if not root: return []
    res, q = [], deque([root])
    while q:
        lvl = []
        for _ in range(len(q)):
            n = q.popleft()
            lvl.append(n.val)
            if n.left: q.append(n.left)
            if n.right: q.append(n.right)
        res.append(lvl)
    return res
```

---

## ✨ **4. Common Interview Problem Patterns**

| Problem                     | Pattern Used     | Key Idea                                                                  |
| --------------------------- | ---------------- | ------------------------------------------------------------------------- |
| **Height / Depth**          | DFS              | `1 + max(left, right)`                                                    |
| **Count Nodes / Sum / Max** | DFS              | Combine children values                                                   |
| **Search / Exists**         | DFS              | `node.val == target or in left or right`                                  |
| **Invert / Mirror**         | DFS + Swap       | Swap left/right post‑DFS                                                  |
| **LCA**                     | DFS              | Return node if matches `p` or `q`; root of split                          |
| **Diameter**                | DFS + Track Max  | Keep max of `left + right` across nodes                                   |
| **BST Search**              | BST property use | Compare and recurse left/right ([YouTube][1], [AlgoMap][2], [YouTube][3]) |

---

## ✍️ **5. Quick Code Snippets**

```python
# Height
def maxDepth(n):
    if not n: return 0
    return 1 + max(maxDepth(n.left), maxDepth(n.right))

# Sum of nodes
def treeSum(n):
    if not n: return 0
    return n.val + treeSum(n.left) + treeSum(n.right)

# Invert tree
def invertTree(n):
    if not n: return None
    n.left, n.right = invertTree(n.right), invertTree(n.left)
    return n

# Search BST
def searchBST(n, target):
    if not n: return False
    return n.val == target or (
        searchBST(n.left, target) if target < n.val else searchBST(n.right, target)
    )
```

---

## ✅ **6. Interview Tips**

* Always **explain base cases** clearly.
* Show **recursive logic** and **combine step**.
* **Dry run** with a small example.
* State **time/space complexities**:

  * *General Tree DFS*: O(N) time, O(h) space.
  * *BST Search*: O(h) avg/O(N) worst-case.

---

## 🧩 **7. Universal Template**

```python
def solve(root):
    if root is None:
        return base_result
    left = solve(root.left)
    right = solve(root.right)
    return combine(root.val, left, right)
```

*Annotate `combine()` based on problem type.*

---

## 📌 **Summary**

Recursion + thoughtful combination = powerful tree solutions. Master DFS/BFS traversals, apply them to standard patterns, and you’ll ace most tree-based interview problems!

---

Let me know if you’d like this styled as a PDF or shareable image version!

[1]: https://www.youtube.com/watch?v=EPwWrs8OtfI&utm_source=chatgpt.com "Binary Trees & Binary Search Trees - DSA Course in Python Lecture 8"
[2]: https://algomap.io/lessons/binary-trees?utm_source=chatgpt.com "Binary Trees & Binary Search Trees | AlgoMap"
[3]: https://www.youtube.com/watch?v=NxbcpaJjm6w&utm_source=chatgpt.com "Nightly News Full Episode – July 19 - YouTube"
