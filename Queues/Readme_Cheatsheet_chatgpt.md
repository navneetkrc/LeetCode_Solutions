# 🎯 Python Queue Cheatsheet

> A complete guide to using **Queues** in Python — essential for coding interviews and real-world development!

---

## 📌 What is a Queue?

A **Queue** is a **FIFO (First In First Out)** data structure.

- Imagine a line at a ticket counter — the first person to join is the first to be served.
- Key operations:
  🟩 `enqueue()` – Add to the rear  
  🟥 `dequeue()` – Remove from the front  
  👀 `peek()` – Front element  
  ❓ `is_empty()` – Check if queue is empty

---

## ✅ Queue Implementations in Python

### 1️⃣ Using `collections.deque` (Recommended)

```python
from collections import deque

queue = deque()

# Enqueue
queue.append(10)
queue.append(20)

# Dequeue
first = queue.popleft()  # ➝ 10

# Peek
front = queue[0]         # ➝ 20

# Check if empty
empty = len(queue) == 0  # ➝ False
````

> 💡 `deque` provides **O(1)** time complexity for both append and popleft.

---

### 2️⃣ Using `queue.Queue` (For multithreading)

```python
from queue import Queue

q = Queue()

q.put(1)       # enqueue
q.put(2)

q.get()        # ➝ 1 (dequeue)
q.qsize()      # ➝ 1
q.empty()      # ➝ False
```

> ⚠️ Use this only when dealing with **thread-safe queues**.

---

### 3️⃣ Using List (❌ Not ideal for dequeue)

```python
queue = []

queue.append('a')       # enqueue
queue.append('b')

queue.pop(0)            # ➝ 'a' (dequeue, O(n) — not recommended)
```

> ⚠️ Avoid using `pop(0)` due to **O(n)** time complexity.

---

## 🔁 Common Queue Operations

| Operation   | `deque`     | `queue.Queue` | Time Complexity |
| ----------- | ----------- | ------------- | --------------- |
| Enqueue     | `append(x)` | `put(x)`      | O(1)            |
| Dequeue     | `popleft()` | `get()`       | O(1)            |
| Peek Front  | `queue[0]`  | N/A           | O(1)            |
| Check Empty | `len()==0`  | `empty()`     | O(1)            |

---

## 🧪 Example: Print Queue Simulation

```python
from collections import deque

def simulate_queue(names):
    queue = deque(names)

    while queue:
        current = queue.popleft()
        print(f"Processing: {current}")

simulate_queue(["Alice", "Bob", "Charlie"])
# ➝ Processing: Alice
# ➝ Processing: Bob
# ➝ Processing: Charlie
```

---

## 📦 Custom Queue Class

```python
class Queue:
    def __init__(self):
        self.items = []

    def enqueue(self, item):
        self.items.append(item)

    def dequeue(self):
        if not self.is_empty():
            return self.items.pop(0)

    def peek(self):
        return self.items[0] if not self.is_empty() else None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)
```

---

## 💼 Queue Use Cases

* **Breadth-First Search (BFS)**
* **Scheduling algorithms**
* **Producer-consumer problems**
* **Data buffering & streaming**
* **Task queues**

---

## 🚀 Interview-Ready Patterns

🧩 Master problems like:

* Implement Circular Queue
* Rotting Oranges (Leetcode)
* Number of Islands (BFS)
* Open the Lock
* Course Schedule (Topological Sort)

> ✍️ Pro Tip: When solving problems, **clearly explain** your enqueue/dequeue logic during interviews.

---

## 🔚 Summary

✅ Use `deque` for most queue tasks
✅ Use `queue.Queue` for threading
❌ Avoid using `list.pop(0)` in production
✅ Practice BFS and simulation problems for interviews

---

🧠 **"Queues are perfect when the order of processing matters — first come, first served!"**

---
