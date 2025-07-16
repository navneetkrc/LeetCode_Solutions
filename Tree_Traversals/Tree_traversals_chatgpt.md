# 🌲 **Tree Traversal Cheatsheet**

## 📌 Traversal Types

| Type        | Order                             |
| ----------- | --------------------------------- |
| Preorder    | Root → Left → Right               |
| Inorder     | Left → Root → Right               |
| Postorder   | Left → Right → Root               |
| Level Order | Top-down, left to right by levels |

---

## ✅ Preorder Traversal (DFS)

### 🔁 Recursive

```python
def preorder(node):
    if not node: return
    result.append(node.val)
    preorder(node.left)
    preorder(node.right)
```

### 🔂 Iterative

```python
def preorder(root):
    stack, result = [root], []
    while stack:
        node = stack.pop()
        if node:
            result.append(node.val)
            stack.append(node.right)
            stack.append(node.left)
    return result
```

---

## ✅ Inorder Traversal (DFS)

### 🔁 Recursive

```python
def inorder(node):
    if not node: return
    inorder(node.left)
    result.append(node.val)
    inorder(node.right)
```

### 🔂 Iterative

```python
def inorder(root):
    stack, result = [], []
    while root or stack:
        while root:
            stack.append(root)
            root = root.left
        root = stack.pop()
        result.append(root.val)
        root = root.right
    return result
```

---

## ✅ Postorder Traversal (DFS)

### 🔁 Recursive

```python
def postorder(node):
    if not node: return
    postorder(node.left)
    postorder(node.right)
    result.append(node.val)
```

### 🔂 Iterative (Modified Preorder + Reverse)

```python
def postorder(root):
    stack, result = [root], []
    while stack:
        node = stack.pop()
        if node:
            result.append(node.val)
            stack.append(node.left)
            stack.append(node.right)
    return result[::-1]
```

---

## ✅ Level Order Traversal (BFS)

### 🔂 Iterative using Queue

```python
from collections import deque
def levelOrder(root):
    if not root: return []
    q, result = deque([root]), []
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        result.append(level)
    return result
```

---

## 🧠 Pro Tips

* Use **stack** for DFS (pre/in/post), and **queue** for BFS (level order).
* **Inorder** of BST gives sorted order.
* For iterative postorder, push `(left, right)` and reverse at end.
* For **zigzag traversal**, alternate direction at each level.

---
