# 🌳 Binary Tree Interview Questions – Infographic Notes

---
<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/511886e7-a037-4a85-ba64-5f2bf1008233" />


---
## 🧠 4-Step Strategy to Solve Any Tree Problem

| Step | Description                                                                |
| ---- | -------------------------------------------------------------------------- |
| 1️⃣ | **Base Case:** What to return for `None` node?                             |
| 2️⃣ | **Left Subtree:** Call the function recursively on `root.left`             |
| 3️⃣ | **Right Subtree:** Call the function recursively on `root.right`           |
| 4️⃣ | **Combine Results:** Combine left/right results for the final return value |

---

## 🔝 Common Binary Tree Interview Patterns

### 1️⃣ Height of Binary Tree

> 📌 **Max depth of tree from root to leaf**

```python
def treeHeight(root):
    if root is None:
        return 0
    leftHeight = treeHeight(root.left)
    rightHeight = treeHeight(root.right)
    return 1 + max(leftHeight, rightHeight)
```

---

### 2️⃣ Check if Value Exists in Tree

> 🔍 **Search for a value recursively**

```python
def existsInTree(root, value):
    if root is None:
        return False
    inLeft = existsInTree(root.left, value)
    inRight = existsInTree(root.right, value)
    return root.data == value or inLeft or inRight
```

---

### 3️⃣ Mirror / Reverse the Tree

> 🔄 **Swap left and right children recursively**

```python
def reverseTree(root):
    if root is None:
        return
    reverseTree(root.left)
    reverseTree(root.right)
    root.left, root.right = root.right, root.left
```

---

### 4️⃣ Sum of All Elements in the Tree

> ➕ **Sum all node values in the binary tree**

```python
def treeSum(root):
    if root is None:
        return 0
    leftSum = treeSum(root.left)
    rightSum = treeSum(root.right)
    return root.data + leftSum + rightSum
```

---

### 5️⃣ Maximum Element in the Tree

> 📈 **Find the largest value node**

```python
def treeMax(root):
    if root is None:
        return float("-inf")
    leftMax = treeMax(root.left)
    rightMax = treeMax(root.right)
    return max(root.data, leftMax, rightMax)
```

---

## ✅ Interviewer Expectations

| What to Explain       | Why It Matters                            |
| --------------------- | ----------------------------------------- |
| Base Case clearly     | Shows you understand recursion boundaries |
| Recursive calls       | Validates tree traversal logic            |
| Combining logic       | Tests how you synthesize results          |
| Dry-run               | Shows clarity on execution order          |
| Time/Space complexity | Displays depth of understanding           |

---

## 🧩 Bonus Tip

🔁 **All tree problems follow a similar recursive pattern.**

Create a template like:

```python
def solve(root):
    if root is None:
        return base_result
    
    left = solve(root.left)
    right = solve(root.right)
    
    return combine(root, left, right)
```

---
Here are **additional Binary Tree questions** that follow the **same recursive 4-step pattern**, ideal for interviews:

---

## 🌳 More Binary Tree Problems with Same Recursive Pattern

### 6️⃣ Count Total Nodes in the Tree

```python
def countNodes(root):
    if root is None:
        return 0
    leftCount = countNodes(root.left)
    rightCount = countNodes(root.right)
    return 1 + leftCount + rightCount
```

---

### 7️⃣ Count Leaf Nodes

```python
def countLeaves(root):
    if root is None:
        return 0
    if root.left is None and root.right is None:
        return 1
    return countLeaves(root.left) + countLeaves(root.right)
```

---

### 8️⃣ Check If Two Trees Are Identical

```python
def isSameTree(p, q):
    if not p and not q:
        return True
    if not p or not q or p.data != q.data:
        return False
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

---

### 9️⃣ Minimum Depth of Binary Tree

```python
def minDepth(root):
    if root is None:
        return 0
    if root.left is None:
        return 1 + minDepth(root.right)
    if root.right is None:
        return 1 + minDepth(root.left)
    return 1 + min(minDepth(root.left), minDepth(root.right))
```

---

### 🔟 Check if Tree is Height Balanced

```python
def isBalanced(root):
    def check(root):
        if root is None:
            return 0, True
        leftHeight, isLeftBalanced = check(root.left)
        rightHeight, isRightBalanced = check(root.right)
        balanced = abs(leftHeight - rightHeight) <= 1 and isLeftBalanced and isRightBalanced
        return 1 + max(leftHeight, rightHeight), balanced
    return check(root)[1]
```

---

### 🔁 Generic Template (Use for Most Recursive Tree Problems)

```python
def solve(root):
    if root is None:
        return base_case_value
    
    left = solve(root.left)
    right = solve(root.right)
    
    return combine(root.data, left, right)
```

---

