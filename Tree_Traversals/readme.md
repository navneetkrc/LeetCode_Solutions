# Binary Tree Reconstruction from Traversals – Interview‑Ready Guide

> **Problems Covered**
> 
> • **105. Construct Binary Tree from Preorder & Inorder Traversal**
>
> • **106. Construct Binary Tree from Inorder & Postorder Traversal**
>
> • **889. Construct Binary Tree from Preorder & Postorder Traversal**
>
> **Why interviewers love these questions**
>
> ▸ Tests your understanding of how different traversals encode structure.
>
> ▸ Reveals your comfort with *divide‑and‑conquer* recursion & pointer arithmetic (array indices).
>
> ▸ Evaluates Big‑O reasoning (*O(n)* build + *O(n)* space) and optimisation decisions (e.g., hash‑map look‑ups).
>
> ▸ Offers follow‑ups: iterative version, handling duplicates, or building with constant extra space.

---

## 1 · LC 105 — Preorder ✚ Inorder ⇒ Tree

### Problem (abridged)

Given the preorder and inorder traversal of a binary tree with **unique** values, rebuild and return the original tree.

### Intuition & Visual

Preorder reveals the **root first**. Inorder sandwiches the root between its left and right subtree.

```text
preorder : [  R | L₁ … Lₖ |  R₁ … Rₘ ]
inorder  : [ L₁ … Lₖ |  R | R₁ … Rₘ ]
             └───┬───┘   └───┬───┘
                 L          R
```

1. Pick the root from preorder `root_value = preorder[0]`.
2. Locate it in inorder (`root_idx`). Everything left of `root_idx` belongs to the left subtree; right side to the right subtree.
3. Recurse on both halves.  ✔️

### Annotated Code (Python ≥ 3.10)

```python
from typing import List, Optional, Dict

class TreeNode:
    def __init__(self, val: int = 0,
                 left: Optional['TreeNode'] = None,
                 right: Optional['TreeNode'] = None):
        self.val, self.left, self.right = val, left, right

class Solution:
    def buildTree(self,
                  preorder: List[int],
                  inorder: List[int]) -> Optional[TreeNode]:
        """Reconstructs the tree in O(n) time / O(n) space."""
        # Build a value→index map so we never call list.index() inside recursion.
        index_map: Dict[int, int] = {value: i for i, value in enumerate(inorder)}

        def helper(pre_start: int, pre_end: int,
                   in_start: int, in_end: int) -> Optional[TreeNode]:
            if pre_start > pre_end:
                return None

            root_value = preorder[pre_start]          # 1️⃣ root from preorder
            root = TreeNode(root_value)
            root_idx = index_map[root_value]          # 2️⃣ split point in inorder
            left_size = root_idx - in_start           # ▶ size of left subtree

            root.left = helper(pre_start + 1,
                               pre_start + left_size,
                               in_start,
                               root_idx - 1)

            root.right = helper(pre_start + left_size + 1,
                                pre_end,
                                root_idx + 1,
                                in_end)
            return root

        return helper(0, len(preorder) - 1, 0, len(inorder) - 1)
```

**Talking Points**

* *Complexity*: `O(n)` time, `O(n)` auxiliary space for the hash‑map + recursion stack.
* Mention why slicing (`preorder[1: …]`) hurts performance (*O(n²)* in worst cases).
* Edge cases: empty tree (`[]`), single‑node tree.
* Follow‑ups: iterative reconstruction with explicit stack.

### Mini Dry‑Run 

| Step | preorder ptrs | inorder window  | Action      |
| ---- | ------------- | --------------- | ----------- |
| 1    | `pre[0]=3`    | `[9 3 15 20 7]` | 3 → root    |
| 2    |               | split @3        | build L & R |

*(Fill in more during the interview; drawing the partition live impresses!)*

---

## 2 · LC 106 — Inorder ✚ Postorder ⇒ Tree

### Key Difference

Postorder lists the **root last** (`Left ➔ Right ➔ Root`). Therefore:

1. Pop the last element as `root_value`.
2. Locate it in inorder to find the right/left subtree sizes.
3. **Build the right subtree first** (because postorder processes it before the root).

### Concise Code

```python
from typing import List, Optional

class Solution:
    def buildTree(self,
                  inorder: List[int],
                  postorder: List[int]) -> Optional[TreeNode]:
        index_map = {v: i for i, v in enumerate(inorder)}

        def helper(in_left: int, in_right: int) -> Optional[TreeNode]:
            if in_left > in_right:
                return None
            root_value = postorder.pop()              # root = last item
            root = TreeNode(root_value)
            root_idx = index_map[root_value]

            # Build right first ➜ mirrors postorder’s sequencing
            root.right = helper(root_idx + 1, in_right)
            root.left  = helper(in_left, root_idx - 1)
            return root
        return helper(0, len(inorder) - 1)
```

**Interview Emphasis**

* Pop from the end keeps `postorder` index state w/o extra variable.
* Highlight symmetry with Problem 105.
* Mention that building *right before left* avoids extra state.

---

## 3 · LC 889 — Preorder ✚ Postorder ⇒ Tree (Ambiguous)

### Why Multiple Answers Exist

Preorder gives `Root ➔ Left‐… ➔ Right‐…`, Postorder gives `…Left ➔ Right ➔ Root`, **but neither reveals the in‑order split location**. Unless the tree is a *full binary tree* (each node has 0 or 2 children) the reconstruction isn’t unique.

### Common Assumption

LeetCode’s test cases ensure uniqueness by using *full* trees and distinct values. State this assumption explicitly to the interviewer.

### Code

```python
from typing import List, Optional

class Solution:
    def constructFromPrePost(self,
                             preorder: List[int],
                             postorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None

        root = TreeNode(preorder[0])
        if len(preorder) == 1:
            return root

        # preorder[1] is always the left child (in a full tree)
        left_child_value = preorder[1]
        left_size = postorder.index(left_child_value) + 1   # inclusive size

        root.left  = self.constructFromPrePost(preorder[1:1+left_size],
                                               postorder[:left_size])
        root.right = self.constructFromPrePost(preorder[1+left_size:],
                                               postorder[left_size:-1])
        return root
```

**Observations to Share**

* If nodes can have a single child, counter‑example exists (draw one!).
* Clarify that the algorithm works because each step confidently picks the left subtree size using the postorder index.

---

## Interview Checklist ✅

| What to verbalise                            | Why it matters                                      |
| -------------------------------------------- | --------------------------------------------------- |
| *Traversal definitions* (pre/in/post)        | Shows basic grounding                               |
| Choosing a hash‑map for O(1) look‑ups        | Demonstrates optimisation instinct                  |
| Avoid list slicing / copying                 | Discuss memory & time efficiency                    |
| Recursion depth = tree height                | Mentions worst‑case stack overflow & tail recursion |
| Edge cases: empty, single node               | Completeness                                        |
| Complexity analysis                          | Always expected                                     |
| Potential follow‑ups (iterative, duplicates) | Shows foresight                                     |

---

## Handy Visual Cue While Explaining

```text
        preorder               inorder
  ┌───────────────┐       ┌───────────────┐
  │  R  L … R …  │       │  L … R  R …  │
  └┬──────────────┘       └──────────────┬┘
   Left subtree size  =  index(R) – start
```

Point at the arrows on a virtual whiteboard (or notebook). Walk the interviewer through one recursive call ➔ partition ➔ two sub‑calls ➔ base case.

---

### Final Takeaways

* Walk through a **small example aloud**; this clarifies the partition logic.
* Emphasise **hash‑map pre‑processing** to avoid quadratic behaviour.
* Remember which subtree to build first (left for 105, *right* for 106, left for 889 under full‑tree assumption).
* Leave a 30‑second buffer at the end to summarise complexity & edge cases.

*Good luck – build those trees & your offer letter!*
