# 🌲 Leetcode 572: Subtree of Another Tree

---

## 📘 Problem Description

**Link:** [Leetcode 572 - Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

> Given the roots of two binary trees `root` and `subRoot`, return `True` if there is a **subtree of `root`** with the **same structure and node values** as `subRoot`, and `False` otherwise.

---

## 🌳 Example

```

Input:
root = [3,4,5,1,2]
subRoot = [4,1,2]

Output: True

```
```

Input:
root = [3,4,5,1,2,null,null,null,null,0]
subRoot = [4,1,2]

Output: False

````

---

## 🧠 Intuition

This is an extension of the **Same Tree** problem.  
To determine if `subRoot` is a subtree of `root`, we:

1. Traverse each node of `root`
2. At every node, check if the subtree starting from that node is **identical** to `subRoot` using a helper function (`sameTree`)
3. If yes — return `True`. Otherwise, recurse on the left and right child.

---

## ✅ Clean Recursive Code with Explanation

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def isSubtree(self, mainRoot: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        # Helper function to check if two trees are identical
        def isSameTree(nodeA, nodeB):
            if not nodeA and not nodeB:
                return True
            if not nodeA or not nodeB:
                return False
            if nodeA.val != nodeB.val:
                return False
            return isSameTree(nodeA.left, nodeB.left) and \
                   isSameTree(nodeA.right, nodeB.right)

        # DFS traversal to find matching subtree
        def dfs(currentNode):
            if not currentNode:
                return False
            if isSameTree(currentNode, subRoot):
                return True
            return dfs(currentNode.left) or dfs(currentNode.right)

        return dfs(mainRoot)
````

---

## 🗣️ What to Explain in Interviews

> “I use DFS to traverse every node in the main tree.
> At each node, I check if the subtree rooted at that node is **identical** to the given `subRoot`.
> For that, I use a helper `isSameTree` function that compares structure and values recursively.”

> “This solution handles nulls, partial matches, and works on unbalanced trees.”

---

## 📈 Time and Space Complexity

| Metric               | Value                                                              |
| -------------------- | ------------------------------------------------------------------ |
| 🕒 Time              | `O(n * m)` — where `n` = nodes in `root`, `m` = nodes in `subRoot` |
| 🧠 Space (Recursion) | `O(h1 + h2)` — height of both trees due to recursion stack         |

---

## 🔁 Alternate Approach: Tree Serialization + Substring Match

You can convert both trees to string using **preorder traversal**, and check if one is a substring of the other.

```python
class Solution:
    def isSubtree(self, s: Optional[TreeNode], t: Optional[TreeNode]) -> bool:
        def serialize(node):
            if not node:
                return "#"
            return f",{node.val},{serialize(node.left)},{serialize(node.right)}"

        return serialize(t) in serialize(s)
```

### ⚠️ Caveat:

You must use delimiters (like `,`) and null markers (`#`) to avoid incorrect matches like:
`[12]` matching `2`.

---

## 🧪 Edge Cases

| Case                                 | Expected                           |
| ------------------------------------ | ---------------------------------- |
| Both `root` and `subRoot` are None   | `True`                             |
| One of them is None                  | `False`                            |
| `subRoot` identical to entire `root` | `True`                             |
| `subRoot` deeper in tree             | `True`/`False` depending on values |

---

## 🧩 Related Problems

* ✅ [100. Same Tree](https://leetcode.com/problems/same-tree/)
* 🔁 [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
* 🔄 [105. Construct Binary Tree from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

---

## 🧠 Final Interview Tips

* Clearly explain the **recursion tree** for `sameTree()`
* Consider cases where tree structure is same but values differ
* Emphasize **pre-checking node values** to avoid extra recursion
* Use tree drawings or dry runs to explain logic

---

