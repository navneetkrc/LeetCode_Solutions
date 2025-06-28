# 📚 Python Queues Cheatsheet

A **Queue** is a linear data structure that follows the **FIFO** (First-In, First-Out) principle. Think of it as a line at a supermarket checkout: the first person to get in line is the first person to be served.

### Core Queue Operations

| Operation   | Description                                     | Analogy                       |
| :---------- | :---------------------------------------------- | :---------------------------- |
| **Enqueue** | ➡️ Add an element to the back (end) of the queue. | A person joining a line.      |
| **Dequeue** | ⬅️ Remove and return the front element.         | The person at the front leaving. |
| **Peek**    | 👀 Return the front element without removing it.  | Looking at who's at the front. |
| **IsEmpty** | ❓ Check if the queue is empty.                 | Is the line empty?            |
| **Size**    | 📏 Get the number of elements in the queue.     | Counting people in the line.  |

---

## 1. Using `collections.deque` (Recommended & Performant)

The `collections.deque` (double-ended queue) is the gold standard for implementing a queue in Python. It's optimized for fast appends and pops from both ends, providing `O(1)` time complexity for enqueue and dequeue operations.

-   **Enqueue**: `deque.append(item)`
-   **Dequeue**: `deque.popleft()`
-   **Peek**: `deque[0]`
-   **IsEmpty**: `not my_deque` or `len(my_deque) == 0`
-   **Size**: `len(my_deque)`

### Basic Usage

```python
from collections import deque

# 1. Initialize an empty queue
queue_deque = deque()
print(f"Is queue empty? {not queue_deque}") # Output: Is queue empty? True

# 2. Enqueue (add) elements
print("\nEnqueuing items...")
queue_deque.append("Alice")  # Front
queue_deque.append("Bob")
queue_deque.append("Charlie") # Back
print(f"Queue: {queue_deque}") # Output: Queue: deque(['Alice', 'Bob', 'Charlie'])

# 3. Peek at the front element
front_element = queue_deque[0]
print(f"Front element (peek): {front_element}") # Output: Front element (peek): Alice
print(f"Queue after peek: {queue_deque}") # Output: Queue after peek: deque(['Alice', 'Bob', 'Charlie'])

# 4. Dequeue (remove) an element
removed_element = queue_deque.popleft()
print(f"Dequeued element: {removed_element}") # Output: Dequeued element: Alice
print(f"Queue after dequeue: {queue_deque}") # Output: Queue after dequeue: deque(['Bob', 'Charlie'])

# 5. Check size and if it's empty
print(f"Current queue size: {len(queue_deque)}") # Output: Current queue size: 2
print(f"Is queue empty? {not queue_deque}") # Output: Is queue empty? False
```

---

## 2. Using `queue.Queue` (Thread-Safe 🔒)

Use this when you need to share a queue between multiple threads in a concurrent program. The methods block by default until the operation can be completed, making it safe for parallel processing.

-   **Enqueue**: `queue.put(item)`
-   **Dequeue**: `queue.get()`
-   **IsEmpty**: `queue.empty()`
-   **Size**: `queue.qsize()`
-   **Peek**: Not directly available. You must `get()` the item and then `put()` it back if needed (not a true peek and tricky in multi-threaded contexts).

### Basic Usage

```python
from queue import Queue

# 1. Initialize a thread-safe queue
queue_safe = Queue(maxsize=3) # Optional: set a max size

# 2. Enqueue (put) elements
print("Putting items into the safe queue...")
queue_safe.put("Task 1")
queue_safe.put("Task 2")
queue_safe.put("Task 3")
# queue_safe.put("Task 4") # This would block (wait) because the queue is full

# 3. Dequeue (get) an element
print(f"Is queue empty? {queue_safe.empty()}") # Output: Is queue empty? False

next_task = queue_safe.get()
print(f"Next task to process: {next_task}") # Output: Next task to process: Task 1
print(f"Current size: {queue_safe.qsize()}") # Output: Current size: 2
```

---

## 3. Using a Python `list` (Simple but Inefficient ⚠️)

You can use a `list` as a queue, but it's **not recommended for performance-sensitive applications**. Popping from the beginning of a list is an `O(N)` operation because all subsequent elements must be shifted.

-   **Enqueue**: `list.append(item)`
-   **Dequeue**: `list.pop(0)` (**Slow!**)
-   **Peek**: `list[0]`

> **Performance Warning:** `list.pop(0)` is inefficient. For any serious queue implementation, always prefer `collections.deque`.

### Basic Usage

```python
# Initialize a queue using a list
queue_list = []

# Enqueue
queue_list.append(100)
queue_list.append(200)
print(f"Queue as list: {queue_list}") # Output: Queue as list: [100, 200]

# Dequeue (the slow part)
removed = queue_list.pop(0)
print(f"Removed item: {removed}") # Output: Removed item: 100
print(f"Queue after pop: {queue_list}") # Output: Queue after pop: [200]
```
---

## 📝 Quick Comparison

| Implementation          | When to Use                                  | Pros                               | Cons                                            |
| :---------------------- | :------------------------------------------- | :--------------------------------- | :---------------------------------------------- |
| **`collections.deque`** | **Most general use cases.** High-performance apps. | Fast `O(1)` enqueue/dequeue, memory efficient. | Needs an import. |
| **`queue.Queue`**       | Multi-threaded or concurrent programming.    | Thread-safe, provides blocking calls. | Slower due to locking overhead, different API.  |
| **`list`**              | Quick tests, learning concepts. (Avoid in production). | Built-in, no import needed.      | **Very slow `O(N)` dequeue**. Not a true queue. |

---

## 💡 Practical Example: A Simple Task Processor

Queues are perfect for managing tasks that need to be executed in order. For example, a print queue or a job scheduler.

**Logic:**
1.  Create a queue to hold tasks.
2.  Add (enqueue) new tasks to the queue as they arrive.
3.  Have a "worker" process that continuously checks the queue.
4.  If the queue is not empty, it dequeues the next task and processes it.

### Code Example

```python
from collections import deque
import time

def task_processor():
    """Simulates processing tasks from a queue in order."""
    task_queue = deque()

    # 1. Add some initial tasks (enqueue)
    print("Adding tasks to the queue...")
    task_queue.append("Send marketing emails")
    task_queue.append("Generate daily report")
    task_queue.append("Backup database")

    print(f"Initial tasks in queue: {list(task_queue)}\n")

    # 2. Process tasks until the queue is empty
    while task_queue:
        current_task = task_queue.popleft() # Dequeue
        print(f"Processing task: '{current_task}'...")
        time.sleep(1) # Simulate work being done
        print(f"  -> Finished '{current_task}'!\n")

    print("All tasks have been processed. The queue is empty.")

# --- Run the simulation ---
task_processor()
```
