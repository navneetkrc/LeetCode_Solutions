# 🔗 Leetcode 23 – Merge K Sorted Lists (Interview Guide)

**Difficulty:** Hard  
**Topic Tags:** Linked List, Heap, Divide & Conquer  
**Asked by:** Google, Amazon, Bloomberg, Microsoft

---

## 🧩 Problem Statement

Merge `k` sorted linked lists and return it as one sorted linked list.  
Each linked list is sorted in ascending order.

```python
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
````

---

## ✅ What Interviewers Expect

1. **Multiple ways** to solve (Min-Heap, Merge Two at a Time, Divide & Conquer).
2. **Trade-offs** between time and space.
3. Can you write **clean, modular code**?
4. Do you know **how and when** to use a **min-heap**?
5. Can you **optimize beyond brute-force**?

---

## ⚙️ Approach 1: Min-Heap (Optimal & Most Common in Interviews)

### 💡 Idea

* Use a min-heap to track the current smallest node across all lists.
* Every time we pop the smallest, we push the next from that list (if it exists).

### ⛓️ Why It Works

At any time, the heap contains at most `k` elements (one from each list).

### 🔢 Time Complexity

* **Time:** `O(N log k)` where `N` is total nodes, `k` is number of lists.
* **Space:** `O(k)` for the heap.

---

### 🧱 Min-Heap Code (Full)

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
    def __lt__(self, other):
        return id(self) < id(other)  # for tie-breaking in heap

class Solution:
    def mergeKLists(self, lists):
        heap = []
        for i, node in enumerate(lists):
            if node:
                heapq.heappush(heap, (node.val, i, node)) 

        dummy = ListNode(0)
        tail = dummy

        while heap:
            val, i, node = heapq.heappop(heap)
            tail.next = node
            tail = tail.next
            if node.next:
                heapq.heappush(heap, (node.next.val, i, node.next)) # i is needed to know about the index of list that needs to be checked for list.next, while popping from heap

        return dummy.next
```

---

## 🧠 Other Approaches (Concise for Interview Notes)

### ⚙️ Approach 2: Brute Force (Collect & Sort)

* Collect all values, sort, and reconstruct the list.
* Easy to code but not space or time optimal.

```python
def mergeKLists(lists):
    nodes = []
    for lst in lists:
        while lst:
            nodes.append(lst.val)
            lst = lst.next
    nodes.sort()

    dummy = ListNode(0)
    curr = dummy
    for val in nodes:
        curr.next = ListNode(val)
        curr = curr.next
    return dummy.next
```

* **Time:** `O(N log N)`
* **Space:** `O(N)`

---

### ⚙️ Approach 3: Merge Two Lists at a Time (Iterative)

* Merge the first two lists, then merge the result with the third, and so on.

```python
def mergeKLists(lists):
    def merge(l1, l2):
        dummy = ListNode()
        tail = dummy
        while l1 and l2:
            if l1.val < l2.val:
                tail.next, l1 = l1, l1.next
            else:
                tail.next, l2 = l2, l2.next
            tail = tail.next
        tail.next = l1 or l2
        return dummy.next

    result = None
    for lst in lists:
        result = merge(result, lst)
    return result
```

* **Time:** `O(kN)`
* **Space:** `O(1)`

---

### ⚙️ Approach 4: Divide and Conquer (Recursive / Bottom-Up)

* Like merge sort: pairwise merge lists until one remains.

```python
def mergeKLists(lists):
    def merge(l1, l2):
        dummy = ListNode()
        tail = dummy
        while l1 and l2:
            if l1.val < l2.val:
                tail.next, l1 = l1, l1.next
            else:
                tail.next, l2 = l2, l2.next
            tail = tail.next
        tail.next = l1 or l2
        return dummy.next

    if not lists: return None
    while len(lists) > 1:
        merged = []
        for i in range(0, len(lists), 2):
            l1 = lists[i]
            l2 = lists[i+1] if i+1 < len(lists) else None
            merged.append(merge(l1, l2))
        lists = merged
    return lists[0]
```

* **Time:** `O(N log k)`
* **Space:** `O(1)` (iterative version), `O(log k)` for recursion

---

## 📈 Visual: Min-Heap in Action

```
Input Lists:
List 1: 1 -> 4 -> 5  
List 2: 1 -> 3 -> 4  
List 3: 2 -> 6

Initial Min-Heap:
[(1, 0, node1), (1, 1, node2), (2, 2, node3)]

Step-by-step:
→ Pop (1, 0), push (4 from List 0)  
→ Pop (1, 1), push (3 from List 1)  
→ Pop (2, 2), push (6 from List 2)  
...
```

---

## 🧠 Interview Tips

* Clarify input: empty lists? nulls? all lists empty?
* Mention why `__lt__` or `(val, index, node)` is needed to make heap work.
* Always check if `node.next` exists before pushing.
* Stress on **time and space** trade-offs between approaches.
* Mention Python's `heapq` is a **min-heap** by default.

---

## 🔚 Summary

| Approach           | Time       | Space    | Suitable For                |
| ------------------ | ---------- | -------- | --------------------------- |
| ✅ Min Heap         | O(N log k) | O(k)     | **Optimal**, Asked in FAANG |
| Brute Force        | O(N log N) | O(N)     | Quick prototype             |
| Iterative Merge    | O(kN)      | O(1)     | Simpler logic               |
| Divide and Conquer | O(N log k) | O(log k) | Elegant + Efficient         |

---

🧾 Practice writing the **Min Heap** and **Divide & Conquer** variants from scratch. These are the most commonly expected.

---
Below is a compact **cheat-sheet** for *Merge K Sorted Lists* plus a ready-to-import **Anki deck**.

### 📝 Quick Algorithm Line-Up

| Approach                 | Idea in 1 line                         | Time           | Space                               | When to Mention       |
| ------------------------ | -------------------------------------- | -------------- | ----------------------------------- | --------------------- |
| **Min-Heap** (canonical) | Keep a heap of current heads, pop/push | **O(N log k)** | O(k)                                | First choice, optimal |
| Divide & Conquer         | Pair-merge like merge-sort             | **O(N log k)** | O(1) iterative / O(log k) recursive | Elegant alt.          |
| Sequential Merge         | Merge lists one by one                 | O(k N)         | O(1)                                | Simpler, slower       |
| Brute Force              | Dump values, sort, rebuild             | O(N log N)     | O(N)                                | Mention as baseline   |

> **Tip:** In Python, store `(val, idx, node)` in the heap and add
> `__lt__ = lambda self, other: id(self) < id(other)` inside `ListNode` to break ties.

---

### 🧩 Min-Heap Steps

1. **Init heap** with head of each non-empty list.
2. While heap not empty:

   1. Pop smallest `(val, i, node)`.
   2. Append `node` to output.
   3. If `node.next`: push `(node.next.val, i, node.next)`.
3. Return `dummy.next`.

---

### 🔑 Must-Know Complexities

* **Optimal:** `O(N log k)` time, `O(k)` space.
* `N` = total nodes, `k` = number of lists.
* Heap ops are `log k`; you perform one per node.

---

### 💬 Talking Points for Interviews

* Why heap beats sequential merge (`log k` vs. `k`).
* Dummy sentinel node usage.
* Edge cases: empty array, single list, lists with duplicate values.
* Tie-breaking in Python heaps & custom `__lt__`.
* Trade-offs across approaches (speed vs. space vs. code clarity).

---

## 📚 Anki Flashcards

### 📝 Quick Algorithm Line-Up

| Approach                 | Idea in 1 line                         | Time           | Space                               | When to Mention       |
| ------------------------ | -------------------------------------- | -------------- | ----------------------------------- | --------------------- |
| **Min-Heap** (canonical) | Keep a heap of current heads, pop/push | **O(N log k)** | O(k)                                | First choice, optimal |
| Divide & Conquer         | Pair-merge like merge-sort             | **O(N log k)** | O(1) iterative / O(log k) recursive | Elegant alt.          |
| Sequential Merge         | Merge lists one by one                 | O(k N)         | O(1)                                | Simpler, slower       |
| Brute Force              | Dump values, sort, rebuild             | O(N log N)     | O(N)                                | Mention as baseline   |

---

