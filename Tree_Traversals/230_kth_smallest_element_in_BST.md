# 🌳 Leetcode 230: Kth Smallest Element in a BST

---

## 🧩 Problem Statement

Given the `root` of a Binary Search Tree (BST), and an integer `k`, return **the kth smallest element** in the BST.

A BST's **in-order traversal** gives nodes in **sorted order**.

### 🔧 Constraints:
- 1 ≤ k ≤ total number of nodes
- Node values are unique and non-negative

---

## 🎯 What Interviewers Expect

✅ Understand BST property  
✅ Discuss traversal methods (recursive vs iterative)  
✅ Explain why in-order gives sorted order  
✅ Write clean and modular code  
✅ Suggest optimizations if modifications (insert/delete) are frequent  

---

## ✅ Approach 1: Recursive In-order Traversal (DFS)

### 🔍 Observations:
- In-order traversal gives values in ascending order.
- Use a counter to track the `kth` element.
- Use mutable containers to hold shared state across recursive calls.

### 👨‍💻 Code:

```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        remaining = [k]           # Tracks how many more nodes to skip
        result = [0]              # Stores the kth smallest value once found

        def in_order_traverse(node: Optional[TreeNode]):
            if not node:
                return

            in_order_traverse(node.left)

            if remaining[0] == 1:
                result[0] = node.val
                return
            remaining[0] -= 1

            in_order_traverse(node.right)

        in_order_traverse(root)
        return result[0]
````

---

## 🔁 Approach 2: Iterative In-order Traversal (Stack)

### 🧠 Insight:

We simulate in-order traversal using a stack and stop after `k` pops.

### 👨‍💻 Code:

```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        current = root

        while True:
            while current:
                stack.append(current)
                current = current.left

            current = stack.pop()
            k -= 1
            if k == 0:
                return current.val
            current = current.right
```

---

## 📈 Complexity Analysis

| Approach      | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Recursive DFS | O(H + k)        | O(H)             |
| Iterative     | O(H + k)        | O(H)             |

H = height of tree (log N for balanced, N for skewed)

---

## 🌳 Dry Run Example

### Tree:

```
      5
     / \
    3   6
   / \
  2   4
 /
1
```

In-order: `[1, 2, 3, 4, 5, 6]`
k = 3 → **Output: 3**

---

## ❓ Follow-up: If the BST is Modified Frequently?

### 💬 Q: How to optimize `kthSmallest()` when:

* Tree has frequent **insert** and **delete**
* You perform **many kth queries**

---

## ✅ Optimized Approach: **Augmented BST with Subtree Counts**

### 🔧 Idea:

Each node maintains:

* `val`: value
* `left`, `right`
* `count`: number of nodes in its subtree

---

### 🧠 Retrieval Logic:

Let `left_count = node.left.count if node.left else 0`

* If `k == left_count + 1`: return `node.val`
* If `k <= left_count`: search in left subtree
* Else: search right with `k - (left_count + 1)`

---

### 👨‍💻 Pseudocode:

```python
class AugmentedNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.count = 1  # total nodes in subtree including self

class AugmentedBST:
    def insert(self, node, val):
        if not node:
            return AugmentedNode(val)

        if val < node.val:
            node.left = self.insert(node.left, val)
        else:
            node.right = self.insert(node.right, val)

        node.count = 1 + (node.left.count if node.left else 0) + \
                         (node.right.count if node.right else 0)
        return node

    def kth_smallest(self, node, k):
        left_count = node.left.count if node.left else 0

        if k == left_count + 1:
            return node.val
        elif k <= left_count:
            return self.kth_smallest(node.left, k)
        else:
            return self.kth_smallest(node.right, k - left_count - 1)
```

---

## 🆚 When to Use Which Approach

| Scenario                             | Best Approach                  |
| ------------------------------------ | ------------------------------ |
| Static Tree                          | Recursive / Iterative In-order |
| Frequent kth queries + insert/delete | Augmented BST with counts      |
| Value range known & dense            | Segment Tree / BIT             |
| Large data with rebalancing needs    | AVL / Red-Black Tree with rank |

---

## 📌 Interview Summary

| Point             | Description                                          |
| ----------------- | ---------------------------------------------------- |
| ✅ Basic Traversal | In-order gives sorted BST                            |
| ✅ Stop Early      | Use `k` counter to halt traversal                    |
| ✅ Tradeoffs       | Recursive is simple, iterative avoids stack overflow |
| ✅ Dynamic Queries | Use Augmented Tree with subtree counts               |
| ✅ Pro Tip         | Use AVL/RB Trees to maintain balance during updates  |

---

## 🧠 Common Pitfalls

* Not decrementing `k` correctly.
* Not returning early after finding the kth node.
* Forgetting BST property that enables in-order = sorted order.

---

## 🧪 Practice Variants

* kth **largest**? → Use **reverse in-order** (Right → Root → Left)
* kth smallest in an **unsorted binary tree**? → Use heap or quickselect

---
