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

### Visual Example

```text
Input:
  preorder = [3,9,20,15,7]      (Root → Left → Right)
  inorder  = [9,3,15,20,7]      (Left → Root → Right)

         3
       /   \
      9     20
           /  \
         15    7
```

### Core Logic (Recursion Step Only)

```python
root_val = preorder[pre_start]               # Root from preorder
mid = index_map[root_val]                    # Locate in inorder
left_size = mid - in_start                   # Determine left subtree size

# Recurse
root.left  = helper(pre_start + 1, pre_start + left_size, in_start, mid - 1)
root.right = helper(pre_start + left_size + 1, pre_end, mid + 1, in_end)
```

### Intuition

* **Preorder gives root first** → fix the root.
* **Inorder splits tree at root** → size of left subtree is `(mid - in_start)`.
* Left subtree in preorder starts from `pre_start + 1`.

---

## 2 · LC 106 — Inorder ✚ Postorder ⇒ Tree

### Visual Example

```text
Input:
  inorder    = [9,3,15,20,7]      (Left → Root → Right)
  postorder = [9,15,7,20,3]      (Left → Right → Root)

         3
       /   \
      9     20
           /  \
         15    7
```

### Core Logic (Recursion Step Only)

```python
root_val = postorder.pop()                   # Root is last
mid = index_map[root_val]                    # Locate in inorder

# Recurse (RIGHT FIRST!)
root.right = helper(mid + 1, in_end)
root.left  = helper(in_start, mid - 1)
```

### Intuition

* Postorder ends with the root.
* Reverse order of traversal → **build right before left**.
* Use hashmap for fast splits.

---

## 3 · LC 889 — Preorder ✚ Postorder ⇒ Tree (Ambiguous)

### Visual Example (Unique Tree Assumption)

```text
Input:
  preorder  = [1,2,4,5,3,6,7]      (Root → Left → Right)
  postorder = [4,5,2,6,7,3,1]      (Left → Right → Root)

         1
       /   \
      2     3
     / \   / \
    4   5 6   7
```

### Core Logic (Recursion Step Only)

```python
root_val = preorder[0]
left_root = preorder[1]
left_size = postorder.index(left_root) + 1

# Recurse using known left subtree size
root.left  = build(preorder[1:left_size+1], postorder[:left_size])
root.right = build(preorder[left_size+1:], postorder[left_size:-1])
```

### Intuition

* Second value in preorder is left child.
* Find its position in postorder → gives size of left subtree.
* Assumes **full binary tree** (0 or 2 children).

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

### Summary Visual

```text
       preorder               inorder
  ┌───────────────┐       ┌───────────────┐
  │  R  L … R …  │       │  L … R  R …  │
  └┬──────────────┘       └──────────────┬┘
   Left subtree size  =  index(R) – start
```

---

### Final Takeaways

* Walk through a **small example aloud**; this clarifies the partition logic.
* Emphasise **hash‑map pre‑processing** to avoid quadratic behaviour.
* Remember which subtree to build first (left for 105, *right* for 106, left for 889 under full‑tree assumption).
* Leave a 30‑second buffer at the end to summarise complexity & edge cases.

*Good luck – build those trees & your offer letter!*
