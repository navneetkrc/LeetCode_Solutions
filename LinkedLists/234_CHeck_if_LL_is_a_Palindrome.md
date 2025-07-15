# 🧩 Problem 234: Palindrome Linked List

## 🔗 LeetCode Link

[🔗 Palindrome Linked List – LeetCode](https://leetcode.com/problems/palindrome-linked-list/)

---
## 📘 Problem Description

Given the `head` of a singly linked list, **return `true` if the list is a palindrome**, otherwise return `false`.

---

### 🔢 Examples

**Example 1:**
```

Input:  head = \[1, 2, 2, 1]
Output: true

```

**Example 2:**
```

Input:  head = \[1, 2]
Output: false

```

---

### ✅ Constraints

- The number of nodes in the list is in the range `[1, 10⁵]`
- `0 <= Node.val <= 9`

---

## 💡 Key Observations for Interviews

- A palindrome reads the same forward and backward.
- You **cannot** use extra space (like converting to array) in the optimal solution.
- You must traverse and manipulate the list in-place to achieve `O(1)` space.

---

## 👨‍🏫 What Should You Explain in the Interview?

### Step-by-step Thought Process:
1. "I'll find the middle of the list using slow and fast pointers."
2. "I will reverse the second half of the list."
3. "Then I'll compare the first half and the reversed second half."
4. "Optional: I can restore the list afterward."

### Key Points to Mention:
- Use **two-pointer technique** to find the middle node.
- Use **in-place reversal** to save space.
- Clearly handle odd vs even number of nodes.
- Always check for **edge cases**: 1 node, 2 nodes, etc.

---

## ✨ Visual Illustration

```

Original:   1 → 2 → 2 → 1
↑
Reverse second half → 1 → 2
Compare:   1 == 1 ✅, 2 == 2 ✅ → Palindrome ✔️

````

---

## ✅ Optimal Code (In-Place Reversal)

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if not head or not head.next:
            return True  # Single node is always a palindrome

        # Step 1: Find the middle of the list
        slow_pointer = head
        fast_pointer = head
        while fast_pointer and fast_pointer.next:
            slow_pointer = slow_pointer.next
            fast_pointer = fast_pointer.next.next

        # Step 2: Reverse the second half
        def reverseList(node):
            previous = None
            current = node
            while current:
                next_node = current.next
                current.next = previous
                previous = current
                current = next_node
            return previous  # New head of reversed list

        reversed_second_half = reverseList(slow_pointer)

        # Step 3: Compare both halves
        pointer1 = head
        pointer2 = reversed_second_half
        while pointer2:
            if pointer1.val != pointer2.val:
                return False
            pointer1 = pointer1.next
            pointer2 = pointer2.next

        return True
````

---

## ⏱️ Time and Space Complexity

| Metric           | Value           |
| ---------------- | --------------- |
| Time Complexity  | O(n)            |
| Space Complexity | O(1) (in-place) |

---

## 🔁 Alternate Approach (Using Extra Space)

You may mention this, but not prefer it:

```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        values = []
        while head:
            values.append(head.val)
            head = head.next
        return values == values[::-1]
```

### 🧠 When to Mention:

* When interviewer explicitly allows O(n) space
* As a first brute-force idea before optimizing

---

## 📌 Edge Cases

* ✅ Single node: `[1]` → palindrome
* ❌ Two distinct nodes: `[1, 2]` → not a palindrome
* ✅ Even length: `[1, 2, 2, 1]` → palindrome
* ✅ Odd length: `[1, 2, 3, 2, 1]` → palindrome

---

## 🧠 Bonus Interview Tips

* Clarify whether you’re allowed to mutate the list.
* If time permits, restore the list after reversing it.
* Talk through your pointer updates out loud.
* Always test on odd/even length and edge input cases.

---

