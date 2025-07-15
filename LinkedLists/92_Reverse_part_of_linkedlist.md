
# 🧩 Problem 92: Reverse Linked List II

## 🔗 LeetCode Link

[🔗 Reverse Linked List II - LeetCode](https://leetcode.com/problems/reverse-linked-list-ii/)

---


## 📘 Problem Description

Given the head of a singly linked list and two integers `left` and `right` where `left <= right`, **reverse the nodes of the list from position `left` to position `right`**, and return the modified list.

---

### 🔢 Examples

**Example 1:**

```

Input:  head = \[1, 2, 3, 4, 5], left = 2, right = 4
Output: \[1, 4, 3, 2, 5]

```

**Example 2:**

```

Input:  head = \[5], left = 1, right = 1
Output: \[5]

````

---

### ✅ Constraints

- The number of nodes in the list is `n`
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

---

## 💡 Key Observations for Interviews

- Reversal must happen **in-place** between nodes at positions `left` and `right`.
- It's important to **maintain the original structure** outside this sublist.
- Handling edge cases like:
  - `left == 1` (i.e., head of the list is part of the reversal)
  - `left == right` (i.e., no actual change needed)

---

## 👨‍🏫 What Should You Say in the Interview?

1. **Clarify the Problem**:  
   - "Are positions 1-based or 0-based?" → They're 1-based.
   - "Should we return a new list or modify the existing one in-place?" → Modify in-place.

2. **Outline Your Plan**:
   - "I will traverse the list to the node just before `left`."
   - "Then reverse the sublist from `left` to `right` using iterative pointer reversal."
   - "Finally, connect the reversed sublist back to the original list."

3. **Mention Edge Cases**:
   - "What if the reversal includes the head or tail?"
   - "What if no reversal is needed?"

---

## ✨ Visual Illustration

Before reversal:  
`1 → 2 → 3 → 4 → 5`, `left = 2`, `right = 4`

After reversal:  
`1 → 4 → 3 → 2 → 5`

---

## ✅ Final Interview-Ready Code (Python)

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode:
        # Edge case: no reversal needed
        if left == right:
            return head

        # Create a dummy node to handle edge cases easily (like reversing from head)
        dummy = ListNode(0)
        dummy.next = head

        # Step 1: Reach node before the 'left' position
        prev = dummy
        for _ in range(left - 1):
            prev = prev.next

        # 'start' is the first node to be reversed
        start = prev.next
        # 'then' is the node after 'start' that will be reversed
        then = start.next

        # Step 2: Reverse the sublist from 'left' to 'right'
        for _ in range(right - left):
            start.next = then.next  # Remove 'then' from its current position
            then.next = prev.next   # Insert 'then' after 'prev'
            prev.next = then        # Link 'prev' to 'then'
            then = start.next       # Move 'then' one step forward

        return dummy.next
````

---

## ⏱️ Time and Space Complexity

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |

We traverse the list once and reverse nodes in-place.

---

## 🔁 Alternate Approach: Using Recursion

This is *less efficient* and **not preferred** for interviews due to stack usage and complexity, but worth knowing:

```python
# Not recommended in interviews
```

---

## 🧠 Interview Tips

* Talk through pointer movements step-by-step. Diagrams help!
* Focus on **why you create a dummy node** — it simplifies edge cases.
* Rehearse dry-runs on paper to make the logic rock-solid.

---


