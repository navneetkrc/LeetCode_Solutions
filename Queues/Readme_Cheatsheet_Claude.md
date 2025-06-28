# 🐍 Python Queues Cheatsheet

> **Queue**: A First-In-First-Out (FIFO) data structure where elements are added at one end (rear) and removed from the other end (front)

---

## 📚 What is a Queue?

A queue follows the **FIFO principle** - the first element added is the first one to be removed, like a line of people waiting where the first person in line is the first to be served.

### Key Operations:
- **Enqueue**: Add element to rear/back
- **Dequeue**: Remove element from front
- **Front/Peek**: View front element without removing
- **Rear**: View rear element without removing
- **Empty**: Check if queue is empty

---

## 🛠️ Implementation Methods

### Method 1: Using collections.deque (Recommended)

```python
from collections import deque

# Initialize empty queue
queue = deque()

# Enqueue operations (add to rear)
queue.append(10)
queue.append(20)
queue.append(30)
print(queue)  # deque([10, 20, 30])

# Dequeue operations (remove from front)
front_element = queue.popleft()
print(front_element)  # 10
print(queue)          # deque([20, 30])

# Peek operations
if queue:
    front = queue[0]    # Front element
    rear = queue[-1]    # Rear element
    print(f"Front: {front}, Rear: {rear}")  # Front: 20, Rear: 30

# Check if empty
is_empty = len(queue) == 0
print(f"Is empty: {is_empty}")  # Is empty: False
```

### Method 2: Using queue.Queue (Thread-Safe)

```python
import queue

# Initialize queue
q = queue.Queue()

# Enqueue operations
q.put(10)
q.put(20)
q.put(30)

# Dequeue operations
front_element = q.get()
print(front_element)  # 10

# Check size
print(f"Size: {q.qsize()}")  # Size: 2

# Check if empty
is_empty = q.empty()
print(f"Is empty: {is_empty}")  # Is empty: False
```

### Method 3: Using List (Not Recommended - Inefficient)

```python
# Initialize queue
queue = []

# Enqueue (add to rear)
queue.append(10)
queue.append(20)
queue.append(30)

# Dequeue (remove from front) - O(n) operation!
front_element = queue.pop(0)  # Inefficient!
print(front_element)  # 10
print(queue)          # [20, 30]
```

### Method 4: Custom Queue Class

```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        """Add item to rear of queue"""
        self.items.append(item)
    
    def dequeue(self):
        """Remove and return item from front of queue"""
        if not self.is_empty():
            return self.items.popleft()
        raise IndexError("Queue is empty")
    
    def front(self):
        """Return front item without removing"""
        if not self.is_empty():
            return self.items[0]
        raise IndexError("Queue is empty")
    
    def rear(self):
        """Return rear item without removing"""
        if not self.is_empty():
            return self.items[-1]
        raise IndexError("Queue is empty")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
    
    def __str__(self):
        return f"Queue({list(self.items)})"

# Usage
queue = Queue()
queue.enqueue(1)
queue.enqueue(2)
queue.enqueue(3)
print(queue)              # Queue([1, 2, 3])
print(queue.dequeue())    # 1
print(queue.front())      # 2
print(queue.rear())       # 3
```

---

## 📋 Quick Reference

| Operation | deque Method | queue.Queue | List Method | Time Complexity |
|-----------|--------------|-------------|-------------|-----------------|
| Enqueue   | `append()`   | `put()`     | `append()`  | O(1)           |
| Dequeue   | `popleft()`  | `get()`     | `pop(0)`    | O(1) / O(n)    |
| Front     | `[0]`        | N/A         | `[0]`       | O(1)           |
| Rear      | `[-1]`       | N/A         | `[-1]`      | O(1)           |
| Size      | `len()`      | `qsize()`   | `len()`     | O(1)           |
| Empty     | `len() == 0` | `empty()`   | `len() == 0`| O(1)           |

---

## 🎯 Queue Types & Implementations

### 1. Simple Queue (FIFO)

```python
from collections import deque

def demonstrate_fifo():
    queue = deque()
    
    # Add customers
    customers = ["Alice", "Bob", "Charlie", "Diana"]
    for customer in customers:
        queue.append(customer)
        print(f"{customer} joined the queue")
    
    print(f"Queue: {list(queue)}")
    
    # Serve customers
    while queue:
        served = queue.popleft()
        print(f"Serving {served}")
        print(f"Remaining: {list(queue)}")

demonstrate_fifo()
```

### 2. Priority Queue

```python
import heapq

class PriorityQueue:
    def __init__(self):
        self.heap = []
        self.counter = 0
    
    def enqueue(self, item, priority):
        # Lower number = higher priority
        heapq.heappush(self.heap, (priority, self.counter, item))
        self.counter += 1
    
    def dequeue(self):
        if self.heap:
            return heapq.heappop(self.heap)[2]
        raise IndexError("Queue is empty")
    
    def is_empty(self):
        return len(self.heap) == 0

# Usage
pq = PriorityQueue()
pq.enqueue("Low priority task", 3)
pq.enqueue("High priority task", 1)
pq.enqueue("Medium priority task", 2)

while not pq.is_empty():
    print(pq.dequeue())
# Output: High priority task, Medium priority task, Low priority task
```

### 3. Circular Queue

```python
class CircularQueue:
    def __init__(self, capacity):
        self.capacity = capacity
        self.queue = [None] * capacity
        self.front = 0
        self.rear = 0
        self.size = 0
    
    def enqueue(self, item):
        if self.size == self.capacity:
            raise OverflowError("Queue is full")
        
        self.queue[self.rear] = item
        self.rear = (self.rear + 1) % self.capacity
        self.size += 1
    
    def dequeue(self):
        if self.size == 0:
            raise IndexError("Queue is empty")
        
        item = self.queue[self.front]
        self.queue[self.front] = None
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return item
    
    def is_full(self):
        return self.size == self.capacity
    
    def is_empty(self):
        return self.size == 0
    
    def __str__(self):
        items = []
        if self.size > 0:
            i = self.front
            for _ in range(self.size):
                items.append(self.queue[i])
                i = (i + 1) % self.capacity
        return f"CircularQueue({items})"

# Usage
cq = CircularQueue(3)
cq.enqueue(1)
cq.enqueue(2)
cq.enqueue(3)
print(cq)           # CircularQueue([1, 2, 3])
print(cq.dequeue()) # 1
cq.enqueue(4)
print(cq)           # CircularQueue([2, 3, 4])
```

---

## 💡 Common Use Cases & Examples

### 1. Breadth-First Search (BFS)

```python
from collections import deque

def bfs_tree_traversal(root):
    if not root:
        return []
    
    queue = deque([root])
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node.value)
        
        # Add children to queue
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return result

# Example with simple node class
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

# Build tree and traverse
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

print(bfs_tree_traversal(root))  # [1, 2, 3, 4, 5]
```

### 2. Task Scheduling

```python
from collections import deque
import time

class TaskScheduler:
    def __init__(self):
        self.task_queue = deque()
    
    def add_task(self, task_name, duration):
        self.task_queue.append((task_name, duration))
        print(f"Added task: {task_name}")
    
    def process_tasks(self):
        while self.task_queue:
            task_name, duration = self.task_queue.popleft()
            print(f"Processing {task_name}...")
            time.sleep(duration)  # Simulate task execution
            print(f"Completed {task_name}")
    
    def show_queue(self):
        print(f"Pending tasks: {[task[0] for task in self.task_queue]}")

# Usage
scheduler = TaskScheduler()
scheduler.add_task("Send emails", 1)
scheduler.add_task("Generate report", 2)
scheduler.add_task("Backup database", 1)
scheduler.show_queue()
# scheduler.process_tasks()  # Uncomment to run
```

### 3. Buffer for Data Streaming

```python
from collections import deque
import random

class StreamBuffer:
    def __init__(self, max_size):
        self.buffer = deque(maxlen=max_size)
        self.max_size = max_size
    
    def add_data(self, data):
        self.buffer.append(data)
        if len(self.buffer) == self.max_size:
            print(f"Buffer full, processing batch: {list(self.buffer)}")
            self.process_batch()
    
    def process_batch(self):
        # Process all data in buffer
        processed = list(self.buffer)
        self.buffer.clear()
        return processed
    
    def get_current_buffer(self):
        return list(self.buffer)

# Usage
buffer = StreamBuffer(5)
for i in range(12):
    data = f"data_{i}"
    buffer.add_data(data)
    print(f"Buffer: {buffer.get_current_buffer()}")
```

### 4. Producer-Consumer Problem

```python
import queue
import threading
import time
import random

def producer(q, producer_id):
    for i in range(5):
        item = f"Item-{producer_id}-{i}"
        q.put(item)
        print(f"Producer {producer_id} produced: {item}")
        time.sleep(random.uniform(0.1, 0.5))

def consumer(q, consumer_id):
    while True:
        try:
            item = q.get(timeout=1)
            print(f"Consumer {consumer_id} consumed: {item}")
            q.task_done()
            time.sleep(random.uniform(0.1, 0.3))
        except queue.Empty:
            break

# Usage
q = queue.Queue(maxsize=10)

# Create threads
producers = [threading.Thread(target=producer, args=(q, i)) for i in range(2)]
consumers = [threading.Thread(target=consumer, args=(q, i)) for i in range(3)]

# Start threads
for p in producers:
    p.start()
for c in consumers:
    c.start()

# Wait for completion
for p in producers:
    p.join()
q.join()
```

---

## ⚠️ Common Pitfalls

### 1. Using List for Queue Operations
```python
# ❌ Bad - O(n) dequeue operation
queue = [1, 2, 3, 4, 5]
first = queue.pop(0)  # This shifts all elements!

# ✅ Good - O(1) operations
from collections import deque
queue = deque([1, 2, 3, 4, 5])
first = queue.popleft()
```

### 2. Not Handling Empty Queue
```python
from collections import deque

# ❌ Bad - causes IndexError
queue = deque()
# item = queue.popleft()  # IndexError!

# ✅ Good - check before dequeuing
if queue:
    item = queue.popleft()
else:
    print("Queue is empty")
```

### 3. Infinite Blocking with queue.Queue
```python
import queue

# ❌ Bad - blocks forever if queue is empty
q = queue.Queue()
# item = q.get()  # Blocks indefinitely!

# ✅ Good - use timeout
try:
    item = q.get(timeout=1)
except queue.Empty:
    print("Queue is empty")
```

---

## 🚀 Advanced Queue Patterns

### 1. Double-Ended Queue (Deque) Operations

```python
from collections import deque

dq = deque([1, 2, 3])

# Add to both ends
dq.appendleft(0)    # Add to front
dq.append(4)        # Add to rear
print(dq)           # deque([0, 1, 2, 3, 4])

# Remove from both ends
front = dq.popleft()  # Remove from front
rear = dq.pop()       # Remove from rear
print(f"Front: {front}, Rear: {rear}")  # Front: 0, Rear: 4
print(dq)             # deque([1, 2, 3])

# Rotate elements
dq.rotate(1)   # Rotate right
print(dq)      # deque([3, 1, 2])
dq.rotate(-2)  # Rotate left
print(dq)      # deque([2, 3, 1])
```

### 2. Queue with Size Limit

```python
from collections import deque

class LimitedQueue:
    def __init__(self, max_size):
        self.max_size = max_size
        self.queue = deque()
    
    def enqueue(self, item):
        if len(self.queue) >= self.max_size:
            self.queue.popleft()  # Remove oldest
        self.queue.append(item)
    
    def dequeue(self):
        if self.queue:
            return self.queue.popleft()
        return None
    
    def __str__(self):
        return f"LimitedQueue({list(self.queue)})"

# Usage - keeps only last 3 items
lq = LimitedQueue(3)
for i in range(6):
    lq.enqueue(i)
    print(lq)
# Output shows only last 3 items: [3, 4, 5]
```

---

## 🎯 Best Practices

1. **Use `collections.deque`** for general queue operations
2. **Use `queue.Queue`** for thread-safe operations
3. **Avoid using lists** for queue operations (inefficient)
4. **Always check for empty queues** before dequeuing
5. **Consider using `maxlen`** for bounded queues
6. **Handle exceptions** gracefully in production code
7. **Use meaningful names** (e.g., `task_queue`, `message_buffer`)

---

## 📊 Performance Comparison

| Implementation | Enqueue | Dequeue | Memory | Thread-Safe |
|----------------|---------|---------|--------|-------------|
| deque          | O(1)    | O(1)    | Low    | No          |
| queue.Queue    | O(1)    | O(1)    | Medium | Yes         |
| list           | O(1)    | O(n)    | Low    | No          |

---

## 📖 Summary

Queues are essential data structures perfect for:
- **Task scheduling** and job processing
- **Breadth-first search** algorithms
- **Buffer management** in streaming
- **Producer-consumer** patterns
- **Print job management**
- **Breadcrumb navigation**

Remember: **First In, First Out** - fair and orderly! 🚶‍♂️➡️🚶‍♀️
