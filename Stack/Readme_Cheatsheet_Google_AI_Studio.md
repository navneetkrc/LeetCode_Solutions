# 📚 Python Stacks Cheatsheet

A **Stack** is a linear data structure that follows the **LIFO** (Last-In, First-Out) principle. Think of it as a stack of plates: you can only add a new plate to the top or remove the topmost plate.

### Core Stack Operations

| Operation | Description                                  | Analogy              |
| :-------- | :------------------------------------------- | :------------------- |
| **Push**  | 🚀 Add an element to the top of the stack.   | Placing a plate on top. |
| **Pop**   | 🪂 Remove and return the top element.         | Taking a plate off.  |
| **Peek**  | 👀 Return the top element without removing it. | Looking at the top plate. |
| **IsEmpty**| ❓ Check if the stack is empty.              | Is the stack of plates empty? |
| **Size**  | 📏 Get the number of elements in the stack.  | Counting the plates. |

---

## 1. Using a Python `list` (Simple & Common)

The easiest way to implement a stack is by using Python's built-in `list`. It's great for simple use cases and learning.

-   **Push**: `list.append(item)`
-   **Pop**: `list.pop()`
-   **Peek**: `list[-1]`
-   **IsEmpty**: `not my_stack` or `len(my_stack) == 0`
-   **Size**: `len(my_stack)`

### Basic Usage

```python
# 1. Initialize an empty stack
stack_list = []
print(f"Is stack empty? {not stack_list}") # Output: Is stack empty? True

# 2. Push elements onto the stack
print("\nPushing items...")
stack_list.append("A") # Bottom
stack_list.append("B")
stack_list.append("C") # Top
print(f"Stack: {stack_list}") # Output: Stack: ['A', 'B', 'C']

# 3. Peek at the top element
top_element = stack_list[-1]
print(f"Peeked at top element: {top_element}") # Output: Peeked at top element: C
print(f"Stack after peek: {stack_list}") # Output: Stack after peek: ['A', 'B', 'C']

# 4. Pop an element from the stack
popped_element = stack_list.pop()
print(f"Popped element: {popped_element}") # Output: Popped element: C
print(f"Stack after pop: {stack_list}") # Output: Stack after pop: ['A', 'B']

# 5. Check size and if it's empty
print(f"Current stack size: {len(stack_list)}") # Output: Current stack size: 2
print(f"Is stack empty? {not stack_list}") # Output: Is stack empty? False
```

---

## 2. Using `collections.deque` (Recommended & Performant)

For performance-critical applications, `collections.deque` (double-ended queue) is the recommended implementation. It's optimized for fast appends and pops from both ends (`O(1)` complexity).

-   **Push**: `deque.append(item)`
-   **Pop**: `deque.pop()`
-   **Peek**: `deque[-1]`
-   **IsEmpty**: `not my_deque` or `len(my_deque) == 0`
-   **Size**: `len(my_deque)`

### Basic Usage

```python
from collections import deque

# 1. Initialize an empty deque
stack_deque = deque()

# 2. Push elements
print("Pushing items to deque...")
stack_deque.append("Apple")
stack_deque.append("Banana")
stack_deque.append("Cherry")
print(f"Stack: {stack_deque}") # Output: Stack: deque(['Apple', 'Banana', 'Cherry'])

# 3. Peek and Pop
top_item = stack_deque[-1]
print(f"Peeked item: {top_item}") # Output: Peeked item: Cherry

popped_item = stack_deque.pop()
print(f"Popped item: {popped_item}") # Output: Popped item: Cherry
print(f"Stack after pop: {stack_deque}") # Output: Stack after pop: deque(['Apple', 'Banana'])
```

---

## 3. Using `queue.LifoQueue` (Thread-Safe 🔒)

Use this when you need to share a stack between multiple threads in a concurrent program. The methods block by default until the operation can be completed.

-   **Push**: `queue.put(item)`
-   **Pop**: `queue.get()`
-   **IsEmpty**: `queue.empty()`
-   **Size**: `queue.qsize()`
-   **Peek**: Not directly available. You must `get()` and then `put()` it back (be careful with this in multi-threaded code).

### Basic Usage

```python
from queue import LifoQueue

# 1. Initialize a thread-safe stack
stack_safe = LifoQueue(maxsize=3) # Optional: set a max size

# 2. Push (put) elements
print("Putting items into LifoQueue...")
stack_safe.put(10)
stack_safe.put(20)
stack_safe.put(30)
# stack_safe.put(40) # This would block if the queue is full

# 3. Pop (get) an element
print(f"Is stack empty? {stack_safe.empty()}") # Output: Is stack empty? False

popped_val = stack_safe.get()
print(f"Got value: {popped_val}") # Output: Got value: 30
print(f"Current size: {stack_safe.qsize()}") # Output: Current size: 2
```

---

## 📝 Quick Comparison

| Implementation          | When to Use                                  | Pros                               | Cons                                            |
| :---------------------- | :------------------------------------------- | :--------------------------------- | :---------------------------------------------- |
| **`list`**              | Simple scripts, learning, non-critical tasks. | Built-in, easy to use, readable.   | Can be slow for very large stacks (reallocation). |
| **`collections.deque`** | **Most general use cases.** High-performance apps. | Fast `O(1)` appends/pops, memory efficient. | Needs an import.                                |
| **`queue.LifoQueue`**   | Multi-threaded or concurrent programming.    | Thread-safe, provides blocking calls. | Slower due to locking overhead, different API.  |

---

## 💡 Practical Example: Balancing Parentheses

A classic use case for a stack is to check if an expression has balanced parentheses `()`, brackets `[]`, and braces `{}`.

**Logic:**
1.  Scan the string from left to right.
2.  If you see an opening bracket (`(`, `[`, `{`), **push** it onto the stack.
3.  If you see a closing bracket (`)`, `]`, `}`), **pop** from the stack. If the popped bracket isn't the corresponding opening bracket, the string is unbalanced.
4.  After the loop, if the stack is **empty**, the string is balanced. Otherwise, it's not.

### Code Example

```python
def is_balanced(expression: str) -> bool:
    """Checks if an expression has balanced brackets using a stack."""
    stack = []
    # Mapping closing brackets to opening ones
    bracket_map = {")": "(", "]": "[", "}": "{"}

    for char in expression:
        if char in bracket_map.values(): # It's an opening bracket
            stack.push(char)
        elif char in bracket_map.keys(): # It's a closing bracket
            if not stack or stack.pop() != bracket_map[char]:
                return False
    
    return not stack # True if stack is empty, False otherwise

# --- Test Cases ---
print(f"'{{[()]}}' is balanced: {is_balanced('{[()]}')}") # Output: '{[()]}' is balanced: True
print(f"'([)]' is balanced: {is_balanced('([)]')}")       # Output: '([)]' is balanced: False
print(f"'(()' is balanced: {is_balanced('(()')}")         # Output: '(()' is balanced: False
```
