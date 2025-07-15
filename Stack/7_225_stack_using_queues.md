
# 📦 Problem 225: Implement Stack using Queues

[🔗 Implement Stack using Queues – LeetCode](https://leetcode.com/problems/implement-stack-using-queues/)

---

## 📘 Problem Description

Implement a **Last-In-First-Out (LIFO) Stack** using only **two queues**.  
Support the following operations just like a regular stack:

### ✅ Required Methods:
- `push(x)` → Push element `x` onto the stack  
- `pop()` → Remove and return the element on top of the stack  
- `top()` → Return the top element  
- `empty()` → Return `True` if the stack is empty, `False` otherwise  

> ⚠️ You may **only use standard queue operations**:  
- enqueue (push to back)  
- dequeue (pop from front)  
- peek front, size, and isEmpty

---

## 🔢 Example Simulation

### Input:
```

["MyStack", "push", "push", "top", "pop", "empty"]
[[],        [1],    [2],    [],     [],     []]

```

### Output:
```

[null, null, null, 2, 2, false]

````

---

## 🎯 Interview Goals

### What Interviewers Expect:
- Do you understand **queue constraints**?
- Can you simulate **stack behavior** using **FIFO structures**?
- Can you choose and justify trade-offs:  
  - ❗ Costly `push` vs. Costly `pop`  
- Are your methods **O(1) amortized** or **O(n)**?

---

## 🧠 Key Observations

- **Queue = FIFO** (First-In-First-Out)  
- **Stack = LIFO** (Last-In-First-Out)  
- You'll need to **manipulate the order** of elements in the queue  
- Two classic ways:
  - ✅ Costly `push`: Rearrange while inserting  
  - ✅ Costly `pop`: Rearrange while removing

---

## ✅ Approach 1: Costly `push` (Preferred for Interview)

### Idea:
- Maintain the most recently pushed element **at the front** of `main_queue`
- Use a **temporary queue** to help reorder

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.main_queue = deque()
        self.helper_queue = deque()

    def push(self, x: int) -> None:
        # Step 1: Enqueue x into helper queue
        self.helper_queue.append(x)
        
        # Step 2: Move everything from main → helper
        while self.main_queue:
            self.helper_queue.append(self.main_queue.popleft())
        
        # Step 3: Swap queues
        self.main_queue, self.helper_queue = self.helper_queue, self.main_queue

    def pop(self) -> int:
        # Top is always at front in this approach
        return self.main_queue.popleft()

    def top(self) -> int:
        return self.main_queue[0]

    def empty(self) -> bool:
        return not self.main_queue
````

---

### ⏱️ Time Complexity

| Operation | Time | Notes                   |
| --------- | ---- | ----------------------- |
| push(x)   | O(n) | Reorders all elements   |
| pop()     | O(1) | Just popleft from front |
| top()     | O(1) | Peek front              |
| empty()   | O(1) | Constant check          |

---

## 🔁 Alternate: Costly `pop` (Less Ideal)

### Idea:

* Always `enqueue` to `main_queue`
* On `pop()`, dequeue everything **except last** and requeue it
* Last element is your `top` / `popped` value

```python
class MyStack:
    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        self.queue.append(x)

    def pop(self) -> int:
        # Leave last element to be returned
        for _ in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())
        return self.queue.popleft()

    def top(self) -> int:
        for _ in range(len(self.queue) - 1):
            self.queue.append(self.queue.popleft())
        result = self.queue[0]
        self.queue.append(self.queue.popleft())
        return result

    def empty(self) -> bool:
        return not self.queue
```

⛔ Not ideal in interviews unless asked to optimize `push()` for O(1)

---

## ✨ Visual Diagram for Costly Push

```
push(1) → main = [1]  
push(2)  
→ helper = [2]  
→ move 1 from main → helper  
→ main = [2, 1]

Top: 2 (front), Pop: 2
```

---

## 🧠 Interview Tips

* Always confirm whether you can use **1 or 2 queues**
* Clearly explain trade-offs of each approach
* Explain **queue swapping** instead of copying back
* Test your implementation mentally with 2–3 operations
* Avoid using `reversed()` or slicing

---
## ✅ Checklist to Cover in the Interview

* [x] Understand how stack differs from queue
* [x] Two approaches: push-heavy vs pop-heavy
* [x] Use only legal queue operations
* [x] Handle all operations (`push`, `pop`, `top`, `empty`)
* [x] Test with empty stack and single element

```
