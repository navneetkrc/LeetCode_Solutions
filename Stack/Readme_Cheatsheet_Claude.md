# 🐍 Python Stacks Cheatsheet

> **Stack**: A Last-In-First-Out (LIFO) data structure where elements are added and removed from the same end (the "top")

---

## 📚 What is a Stack?

A stack follows the **LIFO principle** - the last element added is the first one to be removed, like a stack of plates where you can only add or remove from the top.

### Key Operations:
- **Push**: Add element to top
- **Pop**: Remove element from top  
- **Peek/Top**: View top element without removing
- **Empty**: Check if stack is empty

---

## 🛠️ Implementation Methods

### Method 1: Using Python List (Most Common)

```python
# Initialize empty stack
stack = []

# Push operations
stack.append(10)
stack.append(20)
stack.append(30)
print(stack)  # [10, 20, 30]

# Pop operations
top_element = stack.pop()
print(top_element)  # 30
print(stack)        # [10, 20]

# Peek operation
if stack:
    top = stack[-1]
    print(f"Top element: {top}")  # Top element: 20

# Check if empty
is_empty = len(stack) == 0
print(f"Is empty: {is_empty}")  # Is empty: False
```

### Method 2: Using collections.deque

```python
from collections import deque

# Initialize stack
stack = deque()

# Push operations
stack.append(10)
stack.append(20)
stack.append(30)

# Pop operations
top_element = stack.pop()
print(top_element)  # 30

# More efficient for frequent operations
```

### Method 3: Custom Stack Class

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        raise IndexError("Stack is empty")
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        raise IndexError("Stack is empty")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
    
    def __str__(self):
        return str(self.items)

# Usage
stack = Stack()
stack.push(1)
stack.push(2)
stack.push(3)
print(stack)        # [1, 2, 3]
print(stack.pop())  # 3
print(stack.peek()) # 2
```

---

## 📋 Quick Reference

| Operation | List Method | Time Complexity |
|-----------|-------------|-----------------|
| Push      | `append()`  | O(1)           |
| Pop       | `pop()`     | O(1)           |
| Peek      | `[-1]`      | O(1)           |
| Size      | `len()`     | O(1)           |
| Empty     | `len() == 0`| O(1)           |

---

## 💡 Common Use Cases & Examples

### 1. Balanced Parentheses Checker

```python
def is_balanced(expression):
    stack = []
    pairs = {'(': ')', '[': ']', '{': '}'}
    
    for char in expression:
        if char in pairs:  # Opening bracket
            stack.append(char)
        elif char in pairs.values():  # Closing bracket
            if not stack or pairs[stack.pop()] != char:
                return False
    
    return len(stack) == 0

# Test
print(is_balanced("({[]})"))  # True
print(is_balanced("({[})"))   # False
```

### 2. Reverse a String

```python
def reverse_string(text):
    stack = []
    
    # Push all characters
    for char in text:
        stack.append(char)
    
    # Pop all characters
    reversed_text = ""
    while stack:
        reversed_text += stack.pop()
    
    return reversed_text

print(reverse_string("Hello"))  # "olleH"
```

### 3. Function Call Simulation

```python
def simulate_function_calls():
    call_stack = []
    
    def function_a():
        call_stack.append("function_a")
        print(f"Entering: {call_stack}")
        function_b()
        call_stack.pop()
        print(f"Exiting: {call_stack}")
    
    def function_b():
        call_stack.append("function_b")
        print(f"Entering: {call_stack}")
        function_c()
        call_stack.pop()
        print(f"Exiting: {call_stack}")
    
    def function_c():
        call_stack.append("function_c")
        print(f"Entering: {call_stack}")
        call_stack.pop()
        print(f"Exiting: {call_stack}")
    
    function_a()

simulate_function_calls()
```

### 4. Undo Functionality

```python
class TextEditor:
    def __init__(self):
        self.text = ""
        self.history = []
    
    def type(self, text):
        self.history.append(self.text)  # Save current state
        self.text += text
    
    def undo(self):
        if self.history:
            self.text = self.history.pop()
        else:
            print("Nothing to undo")
    
    def __str__(self):
        return self.text

# Usage
editor = TextEditor()
editor.type("Hello")
editor.type(" World")
print(editor)  # "Hello World"
editor.undo()
print(editor)  # "Hello"
```

---

## ⚠️ Common Pitfalls

### 1. Stack Overflow (Infinite Recursion)
```python
# ❌ Bad - causes stack overflow
def infinite_recursion(n):
    return infinite_recursion(n + 1)

# ✅ Good - with base case
def safe_recursion(n):
    if n <= 0:
        return 0
    return n + safe_recursion(n - 1)
```

### 2. Popping from Empty Stack
```python
# ❌ Bad - causes IndexError
stack = []
# stack.pop()  # IndexError!

# ✅ Good - check before popping
if stack:
    item = stack.pop()
else:
    print("Stack is empty")
```

### 3. Not Handling Edge Cases
```python
def safe_peek(stack):
    if not stack:
        return None  # or raise custom exception
    return stack[-1]
```

---

## 🎯 Best Practices

1. **Always check if stack is empty** before popping or peeking
2. **Use list.append() and list.pop()** for simple stack operations
3. **Consider deque** for high-performance applications
4. **Handle exceptions** gracefully in production code
5. **Use meaningful variable names** (e.g., `call_stack`, `undo_stack`)

---

## 🚀 Advanced: Stack with Maximum

```python
class StackWithMax:
    def __init__(self):
        self.stack = []
        self.max_stack = []
    
    def push(self, item):
        self.stack.append(item)
        if not self.max_stack or item >= self.max_stack[-1]:
            self.max_stack.append(item)
    
    def pop(self):
        if not self.stack:
            return None
        
        item = self.stack.pop()
        if item == self.max_stack[-1]:
            self.max_stack.pop()
        return item
    
    def get_max(self):
        return self.max_stack[-1] if self.max_stack else None

# Usage
stack = StackWithMax()
stack.push(3)
stack.push(1)
stack.push(4)
print(stack.get_max())  # 4
stack.pop()
print(stack.get_max())  # 3
```

---

## 📖 Summary

Stacks are fundamental data structures perfect for:
- **Function calls** and recursion
- **Undo/Redo** operations  
- **Expression evaluation** and syntax parsing
- **Backtracking algorithms**
- **Browser history** navigation

Remember: **Last In, First Out** - that's the stack way! 🥞
