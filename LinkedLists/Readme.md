# 📘 Linked List Interview Preparation Guide (Leetcode Based)

> A comprehensive interview-ready markdown covering top Linked List problems with intuitive explanations, expected observations, and clean, well-commented code.

---

## 🧩 Problem 1: Remove Duplicates from Sorted List — [Leetcode 83](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

### 📝 Problem Description

Given the `head` of a **sorted** linked list, delete all duplicates such that each element appears only once.

### ✅ Key Observations

* Input is **sorted**, so duplicates are **adjacent**.
* One-pass with pointer is sufficient.

### 💡 Interview Expectations

* In-place mutation
* Explain why sorted order helps
* Discuss space: O(1), time: O(n)

### 🔍 Diagram

```
Before: 1 -> 1 -> 2 -> 3 -> 3
         ^
         |
       duplicate

After: 1 -> 2 -> 3
```

### 🧠 Clean Python Code

```python
class Solution:
    def deleteDuplicates(self, head):
        current_node = head
        while current_node and current_node.next:
            if current_node.val == current_node.next.val:
                current_node.next = current_node.next.next  # Skip duplicate
            else:
                current_node = current_node.next  # Move to next node
        return head
```

---

## 🔁 Problem 2: Reverse Linked List — [Leetcode 206](https://leetcode.com/problems/reverse-linked-list/)

### 📝 Problem Description

Reverse a singly linked list.

### ✅ Key Observations

* Requires changing the `next` pointers
* Can be done **iteratively** or **recursively**

### 💡 Interview Expectations

* Time: O(n), Space: O(1) for iterative
* Use meaningful variable names (`prev`, `current`, `next_node`)

### 🔍 Diagram

```
Before: 1 -> 2 -> 3 -> 4
After: 4 -> 3 -> 2 -> 1
```

### 🧠 Iterative Code (Preferred)

```python
class Solution:
    def reverseList(self, head):
        previous = None
        current = head

        while current:
            next_node = current.next  # Save next
            current.next = previous   # Reverse
            previous = current        # Move prev
            current = next_node       # Move current

        return previous
```

---

## 🔀 Problem 3: Merge Two Sorted Lists — [Leetcode 21](https://leetcode.com/problems/merge-two-sorted-lists/)

### 📝 Problem Description

Merge two sorted linked lists into one sorted list.

### ✅ Key Observations

* Dummy node helps avoid edge case issues
* Can be done iteratively or recursively

### 💡 Interview Expectations

* Clear understanding of pointer updates
* Time: O(n), Space: O(1)

### 🔍 Diagram

```
List1: 1 -> 3 -> 5
List2: 2 -> 4 -> 6
Merged: 1 -> 2 -> 3 -> 4 -> 5 -> 6
```

### 🧠 Clean Code (Iterative)

```python
class Solution:
    def mergeTwoLists(self, list1, list2):
        dummy = current = ListNode(0)
        while list1 and list2:
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next
        current.next = list1 or list2
        return dummy.next
```

---

## 🔄 Problem 4: Linked List Cycle — [Leetcode 141](https://leetcode.com/problems/linked-list-cycle/)

### 📝 Problem Description

Detect if a cycle exists in a linked list.

### ✅ Key Observations

* Use Floyd's Cycle Detection (slow & fast pointers)

### 💡 Interview Expectations

* Explain why fast==slow implies cycle
* Time: O(n), Space: O(1)

### 🔍 Diagram

```
1 -> 2 -> 3
     ^    |
     |____|
Cycle exists
```

### 🧠 Code

```python
class Solution:
    def hasCycle(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

---

## 🔢 Problem 5: Insert GCD in Linked List — [Leetcode 2807](https://leetcode.com/problems/insert-greatest-common-divisors-in-linked-list/)

### 📝 Problem Description

Insert node with GCD of current and next node between them.

### ✅ Key Observations

* Traverse the list, compute GCD at each step

### 💡 Interview Expectations

* GCD usage, pointer insertions

### 🔍 Diagram

```
Original: 6 -> 9
GCD(6,9)=3 → Insert: 6 -> 3 -> 9
```

### 🧠 Code

```python
from math import gcd

class Solution:
    def insertGreatestCommonDivisors(self, head):
        current = head
        while current and current.next:
            common = gcd(current.val, current.next.val)
            new_node = ListNode(common)
            new_node.next = current.next
            current.next = new_node
            current = new_node.next
        return head
```

---

## 🧼 Problem 6: Remove N-th Node from End — [Leetcode 19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

### 📝 Problem Description

Remove the nth node from the end of a linked list.

### ✅ Key Observations

* Use two-pointer with `n` gap

### 💡 Interview Expectations

* Dummy node to simplify edge cases
* Explain two-pass vs one-pass

### 🔍 Diagram

```
List: 1 -> 2 -> 3 -> 4 -> 5, n = 2
Remove: 4
Result: 1 -> 2 -> 3 -> 5
```

### 🧠 One-pass Code

```python
class Solution:
    def removeNthFromEnd(self, head, n):
        dummy = ListNode(0, head)
        first = second = dummy
        for _ in range(n):
            first = first.next
        while first.next:
            first = first.next
            second = second.next
        second.next = second.next.next
        return dummy.next
```

---

## 📍 Problem 7: Middle of Linked List — [Leetcode 876](https://leetcode.com/problems/middle-of-the-linked-list/)

### 📝 Problem Description

Return the middle node of the linked list.

### ✅ Key Observations

* Fast pointer moves twice as fast

### 💡 Interview Expectations

* Handle even-length lists properly

### 🔍 Diagram

```
List: 1 -> 2 -> 3 -> 4 -> 5
Slow reaches 3 when fast reaches end
```

### 🧠 Code

```python
class Solution:
    def middleNode(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

---

## 🔁 Problem 8: Copy List with Random Pointer — [Leetcode 138](https://leetcode.com/problems/copy-list-with-random-pointer/)

### 📝 Problem Description

Copy a list where each node has a `random` pointer in addition to `next`.

### ✅ Key Observations

* Use a hash map to keep old->new node mapping

### 💡 Interview Expectations

* Deep copy, correct pointer references

### 🔍 Diagram

```
Original:
1 -> 2 -> 3
|    |
V    V
3    1

Copied:
1' -> 2' -> 3'
|      |
V      V
3'     1'
```

### 🧠 Code

```python
class Solution:
    def copyRandomList(self, head):
        if not head:
            return None

        old_to_new = {}
        current = head

        # First pass: create copies
        while current:
            old_to_new[current] = ListNode(current.val)
            current = current.next

        # Second pass: assign next and random
        current = head
        while current:
            old_to_new[current].next = old_to_new.get(current.next)
            old_to_new[current].random = old_to_new.get(current.random)
            current = current.next

        return old_to_new[head]
```

---

## 🧪 Final Interview Tips

* Simulate pointer updates out loud
* Handle edge cases (empty list, one node, cycle, etc.)
* Use dummy nodes where deletion is involved
* Always mention time/space complexity
* Offer both iterative and recursive if time permits

> 💡 Pro Tip: Draw the list and pointers on paper if stuck. Visualizing helps immensely in Linked List problems.

---
