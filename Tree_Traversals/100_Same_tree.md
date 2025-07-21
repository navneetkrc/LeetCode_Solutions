# 🌳 Leetcode 100: Same Tree

---

## 🧩 Problem Description

**Link:** [Leetcode - Same Tree](https://leetcode.com/problems/same-tree/)  
**Difficulty:** Easy

Given the roots of two binary trees `p` and `q`, write a function to check if they are **the same** or not.

Two binary trees are considered the same if:
- They are **structurally identical**, and
- The nodes have the **same value**.

---

## 📘 Example

```

Input:
p = [1,2,3]
q = [1,2,3]

Output: True

Input:
p = [1,2]
q = [1,null,2]

Output: False

````

---

## 🧠 Intuition

## ✅ Interview-Ready Recursive Code

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isSameTree(self, rootA: Optional[TreeNode], rootB: Optional[TreeNode]) -> bool:
        # Case 1: Both nodes are None (leaf level match)
        if not rootA and not rootB:
            return True

        # Case 2: One of the nodes is None (structure mismatch)
        if not rootA or not rootB:
            return False

        # Case 3: Values do not match
        if rootA.val != rootB.val:
            return False

        # Case 4: Recursively check left and right subtrees
        return self.isSameTree(rootA.left, rootB.left) and \
               self.isSameTree(rootA.right, rootB.right)
````

---

## 🗣 What to Say in Interviews

> "I'll use recursion to simultaneously walk both trees.
> At each step, I’ll compare if the current nodes are the same and recursively verify the left and right subtrees.
> If any mismatch occurs — either structurally or in value — I return `False`."

> "I’ve handled the base cases (null nodes, value mismatch) first to keep the logic clean and efficient."

---

## 📈 Complexity Analysis

| Metric   | Value                                                               |
| -------- | ------------------------------------------------------------------- |
| 🕒 Time  | O(min(n, m)) where n and m are number of nodes in trees `p` and `q` |
| 🧠 Space | O(h) for recursion stack, where `h` is the height of the trees      |

---


## 🔍 Edge Cases to Handle

| Case                        | Result  | Why?                     |
| --------------------------- | ------- | ------------------------ |
| Both trees are `None`       | `True`  | Both are empty           |
| One tree is `None`          | `False` | Structure mismatch       |
| Same structure, diff values | `False` | Value mismatch           |
| Identical trees             | `True`  | Structure + values match |

---

## 🧪 Follow-Up Variations

* 🧬 [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
* 🧪 [Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/)
* 🧾 Tree Serialization/Deserialization problems

---

## 🧠 Tip to Mention

> “Recursion mirrors the tree structure, making this problem very natural to solve recursively. But I’m happy to implement it iteratively using a queue if needed.”

---
