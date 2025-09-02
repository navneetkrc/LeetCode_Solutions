# 📘 Python Interview Cheatsheet with Dry-Runs

---

## 🔗 Linked List

### Problem: Reverse a Linked List

```python
def reverseList(head):
    prev, curr = None, head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev, curr = curr, nxt
    return prev
```

#### Example

Input: `1 -> 2 -> 3 -> 4 -> None`

**Dry Run**

| Step | curr | prev | nxt  | List so far                |
| ---- | ---- | ---- | ---- | -------------------------- |
| 1    | 1    | None | 2    | `1 -> None`                |
| 2    | 2    | 1    | 3    | `2 -> 1 -> None`           |
| 3    | 3    | 2    | 4    | `3 -> 2 -> 1 -> None`      |
| 4    | 4    | 3    | None | `4 -> 3 -> 2 -> 1 -> None` |

✅ Output: `4 -> 3 -> 2 -> 1 -> None`

---

## 🏗️ Stack

### Problem: Largest Rectangle in Histogram

```python
def largestRectangleArea(heights):
    stack, max_area = [], 0
    for i, h in enumerate(heights + [0]):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    return max_area
```

#### Example

Input: `[2, 1, 5, 6, 2, 3]`

**Visualization**

* At `5, 6`: stack grows (monotonic increasing).
* When we hit `2`, pop heights `6` and `5` and compute area.

**Dry Run Table**

| i | h | Stack indices | Action                  | Max area |
| - | - | ------------- | ----------------------- | -------- |
| 0 | 2 | \[0]          | push                    | 0        |
| 1 | 1 | \[1]          | pop 2 → area=2          | 2        |
| 2 | 5 | \[1,2]        | push                    | 2        |
| 3 | 6 | \[1,2,3]      | push                    | 2        |
| 4 | 2 | \[1,4]        | pop 6→area=6, 5→area=10 | 10       |
| 5 | 3 | \[1,4,5]      | push                    | 10       |
| 6 | 0 | \[]           | pop 3→area=3, 2→area=8  | 10       |

✅ Output: `10`

---

## 📦 Queue (Deque)

### Problem: Sliding Window Maximum

```python
from collections import deque

def maxSlidingWindow(nums, k):
    dq, res = deque(), []
    for i, num in enumerate(nums):
        while dq and dq[0] <= i - k:
            dq.popleft()
        while dq and nums[dq[-1]] < num:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            res.append(nums[dq[0]])
    return res
```

#### Example

Input: `nums = [1,3,-1,-3,5,3,6,7], k=3`

**Dry Run**

| i | num | dq (indices) | dq values  | Max |
| - | --- | ------------ | ---------- | --- |
| 0 | 1   | \[0]         | \[1]       |     |
| 1 | 3   | \[1]         | \[3]       |     |
| 2 | -1  | \[1,2]       | \[3,-1]    | 3   |
| 3 | -3  | \[1,2,3]     | \[3,-1,-3] | 3   |
| 4 | 5   | \[4]         | \[5]       | 5   |
| 5 | 3   | \[4,5]       | \[5,3]     | 5   |
| 6 | 6   | \[6]         | \[6]       | 6   |
| 7 | 7   | \[7]         | \[7]       | 7   |

✅ Output: `[3,3,5,5,6,7]`

---

## ⛰️ Heap (Priority Queue)

### Problem: Kth Largest Element

```python
import heapq

def findKthLargest(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]
```

#### Example

Input: `nums = [3,2,1,5,6,4], k=2`

**Dry Run**

| Step | Heap     | Action               |
| ---- | -------- | -------------------- |
| 1    | \[3]     | push 3               |
| 2    | \[2,3]   | push 2               |
| 3    | \[1,3,2] | push 1, pop → \[2,3] |
| 4    | \[2,3,5] | push 5, pop → \[3,5] |
| 5    | \[3,5,6] | push 6, pop → \[5,6] |
| 6    | \[4,6,5] | push 4, pop → \[5,6] |

✅ Output: `5`

---

# 🏁 Summary

* **Linked List:** Reverse → two pointers.
* **Stack:** Histogram → monotonic stack.
* **Queue:** Sliding window max → deque for O(n).
* **Heap:** Kth largest → maintain size-k min-heap.

---


---

# 📝 Python Data Structures Cheatsheet (Interview Prep)

---

## 🔗 Linked Lists

### Basic Node Definition

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

### Traversal

```python
def traverse(head):
    curr = head
    while curr:
        print(curr.val, end=" -> ")
        curr = curr.next
```

### Famous Problems

#### 1. Reverse a Linked List

```python
def reverseList(head):
    prev, curr = None, head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev, curr = curr, nxt
    return prev
```

#### 2. Detect Cycle in Linked List

```python
def hasCycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow == fast:
            return True
    return False
```

#### 3. Merge Two Sorted Lists

```python
def mergeTwoLists(l1, l2):
    dummy = ListNode()
    curr = dummy
    while l1 and l2:
        if l1.val < l2.val:
            curr.next, l1 = l1, l1.next
        else:
            curr.next, l2 = l2, l2.next
        curr = curr.next
    curr.next = l1 or l2
    return dummy.next
```

---

## 🏗️ Stacks

### Using Python List

```python
stack = []
stack.append(10)  # push
stack.append(20)
print(stack.pop())  # pop
```

### Famous Problems

#### 1. Valid Parentheses

```python
def isValid(s):
    stack, mapping = [], {')': '(', ']': '[', '}': '{'}
    for ch in s:
        if ch in mapping:
            if not stack or stack.pop() != mapping[ch]:
                return False
        else:
            stack.append(ch)
    return not stack
```

#### 2. Min Stack

```python
class MinStack:
    def __init__(self):
        self.stack, self.min_stack = [], []

    def push(self, x):
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)

    def pop(self):
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self):
        return self.stack[-1]

    def getMin(self):
        return self.min_stack[-1]
```

#### 3. Largest Rectangle in Histogram

```python
def largestRectangleArea(heights):
    stack, max_area = [], 0
    for i, h in enumerate(heights + [0]):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    return max_area
```

---

## 📦 Queues

### Using `collections.deque`

```python
from collections import deque
queue = deque()
queue.append(10)  # enqueue
queue.append(20)
print(queue.popleft())  # dequeue
```

### Famous Problems

#### 1. Sliding Window Maximum

```python
from collections import deque

def maxSlidingWindow(nums, k):
    dq, res = deque(), []
    for i, num in enumerate(nums):
        while dq and dq[0] <= i - k:
            dq.popleft()
        while dq and nums[dq[-1]] < num:
            dq.pop()
        dq.append(i)
        if i >= k - 1:
            res.append(nums[dq[0]])
    return res
```

#### 2. Rotten Oranges (BFS)

```python
def orangesRotting(grid):
    from collections import deque
    rows, cols = len(grid), len(grid[0])
    q = deque()
    fresh = 0
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                q.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh += 1
    
    time = 0
    while q:
        r, c, time = q.popleft()
        for dr, dc in [(1,0),(-1,0),(0,1),(0,-1)]:
            nr, nc = r+dr, c+dc
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2
                fresh -= 1
                q.append((nr, nc, time+1))
    return time if fresh == 0 else -1
```

---

## ⛰️ Heaps (Priority Queues)

### Using `heapq` (min-heap by default)

```python
import heapq
nums = [5, 3, 8]
heapq.heapify(nums)   # O(n)
heapq.heappush(nums, 1)
print(heapq.heappop(nums))  # smallest element
```

### Famous Problems

#### 1. Kth Largest Element

```python
import heapq
def findKthLargest(nums, k):
    return heapq.nlargest(k, nums)[-1]
```

#### 2. Merge K Sorted Lists

```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeKLists(lists):
    heap = []
    for i, l in enumerate(lists):
        if l:
            heapq.heappush(heap, (l.val, i, l))
    dummy = ListNode()
    curr = dummy
    while heap:
        val, i, node = heapq.heappop(heap)
        curr.next, node = node, node.next
        curr = curr.next
        if node:
            heapq.heappush(heap, (node.val, i, node))
    return dummy.next
```

#### 3. Top K Frequent Elements

```python
import heapq
from collections import Counter

def topKFrequent(nums, k):
    freq = Counter(nums)
    return [x for x, _ in heapq.nlargest(k, freq.items(), key=lambda x: x[1])]
```

---

✅ This cheatsheet covers:

* **Linked Lists**: Reverse, Cycle detection, Merge.
* **Stacks**: Valid parentheses, MinStack, Histogram problem.
* **Queues**: Sliding window max, Rotten oranges BFS.
* **Heaps**: Kth largest, Merge K lists, Top K frequent.

---
