# 📚 Python Stack Cheatsheet

> Your go-to guide for mastering **Stacks** using Python!

---

## 📌 What is a Stack?

A **Stack** is a **LIFO (Last In First Out)** data structure.

- Think of a stack of plates — last plate placed is the first one removed.
- Common operations:  
  🔼 `push()` – Add to top  
  🔽 `pop()` – Remove from top  
  👀 `peek()` – Look at top item  
  ❓ `is_empty()` – Check if stack is empty

---

## 🛠️ Ways to Implement a Stack in Python

### ✅ Using a List (Built-in)

```python
stack = []

# Push elements
stack.append(1)
stack.append(2)
stack.append(3)

# Stack now: [1, 2, 3]

# Pop element
top = stack.pop()  # ➝ 3

# Peek at top
top_element = stack[-1]  # ➝ 2

# Check if empty
is_empty = len(stack) == 0
````

---

### ✅ Using `collections.deque` (Preferred for performance)

```python
from collections import deque

stack = deque()

stack.append('a')
stack.append('b')
stack.append('c')

stack.pop()        # ➝ 'c'
stack[-1]          # ➝ 'b'
len(stack) == 0    # ➝ False
```

> 💡 `deque` is optimized for fast `append` and `pop` from both ends (O(1) time).

---

### ❌ Avoid Using `insert(0, x)` or `pop(0)`

They turn your stack into a **queue** and are **O(n)** operations on lists.

---

## 📋 Common Stack Operations

| Operation   | List        | deque       | Time Complexity |
| ----------- | ----------- | ----------- | --------------- |
| Push        | `append(x)` | `append(x)` | O(1)            |
| Pop         | `pop()`     | `pop()`     | O(1)            |
| Peek (Top)  | `stack[-1]` | `stack[-1]` | O(1)            |
| Check Empty | `len()==0`  | `len()==0`  | O(1)            |

---

## 🧪 Example: Reverse a String Using Stack

```python
def reverse_string(s: str) -> str:
    stack = list(s)
    reversed_str = ''

    while stack:
        reversed_str += stack.pop()

    return reversed_str

print(reverse_string("hello"))  # ➝ 'olleh'
```

---

## 🔄 Bracket Matching Example

```python
def is_valid_brackets(expr: str) -> bool:
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}

    for ch in expr:
        if ch in '([{':
            stack.append(ch)
        elif ch in ')]}':
            if not stack or stack[-1] != pairs[ch]:
                return False
            stack.pop()
    
    return not stack

# Test
print(is_valid_brackets("{[()]}"))  # ➝ True
print(is_valid_brackets("{[(])}"))  # ➝ False
```

---

## 🧠 Stack Interview Tips

* Be ready to **simulate stack operations**.
* Master problems like:

  * Valid Parentheses
  * Min Stack
  * Evaluate Reverse Polish Notation
  * Largest Rectangle in Histogram
* Be clear on **when** to use a **stack**:

  * Reversals
  * Undo operations
  * Recursion simulation
  * Expression evaluation

---

## 📦 Bonus: Implementing Your Own Stack Class

```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        return self.items[-1] if not self.is_empty() else None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)
```

---

## 🔚 Summary

✅ Use `list` or `deque` for stacks in Python
✅ Keep operations LIFO (Last In First Out)
✅ Think of use-cases like recursion, parentheses matching, etc.

---

**🧠 Quick Reminder:**
"Stacks are simple but powerful — many tricky problems have elegant stack-based solutions!"

---
