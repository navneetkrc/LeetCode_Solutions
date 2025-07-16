# Tree Traversal Cheatsheet

## Depth-First Search (DFS)

### Pre-order (Root → Left → Right)
```
visit(root)
preorder(left)
preorder(right)
```
**Use case:** Copy tree, prefix expression

### In-order (Left → Root → Right)
```
inorder(left)
visit(root)
inorder(right)
```
**Use case:** BST sorted output, infix expression

### Post-order (Left → Right → Root)
```
postorder(left)
postorder(right)
visit(root)
```
**Use case:** Delete tree, postfix expression, calculate size

## Breadth-First Search (BFS)

### Level-order (Top to Bottom, Left to Right)
```
queue = [root]
while queue:
    node = queue.pop(0)
    visit(node)
    if node.left: queue.append(node.left)
    if node.right: queue.append(node.right)
```
**Use case:** Level-by-level processing, shortest path

## Implementation Patterns

### Recursive DFS
```python
def traverse(node):
    if not node: return
    # Pre-order: process here
    traverse(node.left)
    # In-order: process here  
    traverse(node.right)
    # Post-order: process here
```

### Iterative DFS (Stack)
```python
stack = [root]
while stack:
    node = stack.pop()
    visit(node)
    if node.right: stack.append(node.right)
    if node.left: stack.append(node.left)
```

### Iterative BFS (Queue)
```python
from collections import deque
queue = deque([root])
while queue:
    node = queue.popleft()
    visit(node)
    if node.left: queue.append(node.left)
    if node.right: queue.append(node.right)
```

## Time & Space Complexity
- **Time:** O(n) for all traversals
- **Space:** O(h) recursive, O(w) iterative BFS
  - h = height, w = max width

## Quick Reference
| Traversal | Order | Stack/Queue | Common Use |
|-----------|-------|-------------|------------|
| Pre-order | Root-L-R | Stack | Tree copy |
| In-order | L-Root-R | Stack | BST sort |
| Post-order | L-R-Root | Stack | Tree delete |
| Level-order | Top-Bottom | Queue | BFS |
