# Recursion Problems and Solutions

A comprehensive collection of recursive programming problems with detailed explanations and multiple solution approaches.

---

## Table of Contents

1. [Print Name N Times (Limited to 5)](#1-print-name-n-times-limited-to-5)
2. [Print Numbers 1 to N](#2-print-numbers-1-to-n)
3. [Print Numbers N to 1](#3-print-numbers-n-to-1)
4. [Print 1 to N Using Backtracking](#4-print-1-to-n-using-backtracking)
5. [Print N to 1 Using Backtracking](#5-print-n-to-1-using-backtracking)


---

## 1. Print Name N Times (Limited to 5) 

**Problem:** Write a recursive function that prints a given name n times, but limits the output to a maximum of 5 repetitions.

### Solution

```python
def print_name_recursive(name, n, current=1):
    """
    Print a name n times using recursion (limited to maximum 5 times)
    
    Args:
        name (str): The name to print
        n (int): Number of times to print (will be capped at 5)
        current (int): Current iteration counter (used internally)
    """
    # Base case: stop if we've reached n or exceeded 5
    if current > n or current > 5:
        return
    
    # Print the name with the current number
    print(f"{current}. {name}")
    
    # Recursive call for the next iteration
    print_name_recursive(name, n, current + 1)
```

### Example Usage

```python
print_name_recursive("Alice", 3)
# Output:
# 1. Alice
# 2. Alice
# 3. Alice

print_name_recursive("Bob", 7)  # Limited to 5
# Output:
# 1. Bob
# 2. Bob
# 3. Bob
# 4. Bob
# 5. Bob
```

### Key Concepts
- **Base Case:** `current > n or current > 5`
- **Recursive Case:** Print current iteration and call with `current + 1`
- **Constraint Handling:** Automatic limitation to maximum 5 iterations

---

## 2. Print Numbers 1 to N

**Problem:** Write a recursive function that prints all numbers from 1 to n in ascending order.

### Method 1: Forward Counting

```python
def print_1_to_n(n, current=1):
    """
    Print numbers from 1 to n using recursion
    
    Args:
        n (int): The upper limit (inclusive)
        current (int): Current number being processed (used internally)
    """
    # Base case: stop when current exceeds n
    if current > n:
        return
    
    # Print the current number
    print(current)
    
    # Recursive call for the next number
    print_1_to_n(n, current + 1)
```

### Method 2: Using Call Stack

```python
def print_1_to_n_alternative(n):
    """
    Alternative approach: print from n down to 1, then print on return
    This prints in ascending order due to the recursive call stack
    
    Args:
        n (int): The upper limit (inclusive)
    """
    # Base case: stop when n is less than 1
    if n < 1:
        return
    
    # Recursive call first (goes deeper into recursion)
    print_1_to_n_alternative(n - 1)
    
    # Print on the way back up (after recursive calls return)
    print(n)
```

### Example Usage

```python
print_1_to_n(5)
# Output: 1 2 3 4 5 (each on new line)

print_1_to_n_alternative(5)
# Output: 1 2 3 4 5 (each on new line)
```

### Key Concepts
- **Method 1:** Direct forward progression with helper parameter
- **Method 2:** Clever use of call stack to achieve ascending order
- **Base Case:** Different for each method but both handle n < 1

---

## 3. Print Numbers N to 1

**Problem:** Write a recursive function that prints all numbers from n down to 1 in descending order.

### Method 1: Direct Countdown

```python
def print_n_to_1(n):
    """
    Print numbers from n to 1 using recursion
    
    Args:
        n (int): The starting number (will print from n down to 1)
    """
    # Base case: stop when n is less than 1
    if n < 1:
        return
    
    # Print the current number
    print(n)
    
    # Recursive call with n-1
    print_n_to_1(n - 1)
```

### Method 2: Using Helper Parameter

```python
def print_n_to_1_alternative(n, current=None):
    """
    Alternative approach using a helper parameter
    
    Args:
        n (int): The starting number
        current (int): Current number being processed (used internally)
    """
    # Initialize current to n if not provided
    if current is None:
        current = n
    
    # Base case: stop when current is less than 1
    if current < 1:
        return
    
    # Print the current number
    print(current)
    
    # Recursive call with current-1
    print_n_to_1_alternative(n, current - 1)
```

### Method 3: Using Call Stack for Reverse Order

```python
def print_n_to_1_using_stack(n, target=1):
    """
    Third approach: uses call stack to print in reverse order
    Makes recursive call first, then prints on return
    
    Args:
        n (int): The upper limit
        target (int): Current position (starts from 1, goes up to n)
    """
    # Base case: stop when target exceeds n
    if target > n:
        return
    
    # Recursive call first (goes deeper into recursion)
    print_n_to_1_using_stack(n, target + 1)
    
    # Print on the way back up (prints n, n-1, n-2, ..., 1)
    print(target)
```

### Example Usage

```python
print_n_to_1(5)
# Output: 5 4 3 2 1 (each on new line)
```

### Key Concepts
- **Method 1:** Most intuitive - direct parameter decrementing
- **Method 2:** Explicit tracking with helper parameter
- **Method 3:** Demonstrates call stack manipulation for reverse output

---

## 4. Print 1 to N Using Backtracking

**Problem:** Write a recursive function that prints numbers from 1 to n using the backtracking paradigm.

### Clean Implementation

```python
def print_1_to_n(n, current=1):
    """
    Print numbers from 1 to n using backtracking approach.
    
    Args:
        n (int): Upper limit (inclusive)
        current (int): Current number to print
    """
    # Base case: reached the end
    if current > n:
        return
    
    # Print current number
    print(current)
    
    # Recursive call (explore next)
    print_1_to_n(n, current + 1)
    
    # Backtrack point (implicit return)
```

### Backtracking Visualization

```python
def demonstrate_backtracking(n, current=1, indent=0):
    """
    Demonstrates the backtracking process with visual indentation.
    
    Args:
        n (int): Upper limit
        current (int): Current number
        indent (int): Indentation level for visualization
    """
    spaces = "  " * indent
    
    # Base case
    if current > n:
        print(f"{spaces}Base case reached, returning...")
        return
    
    # Forward: make choice and explore
    print(f"{spaces}→ Processing {current}")
    demonstrate_backtracking(n, current + 1, indent + 1)
    
    # Backward: backtrack
    print(f"{spaces}← Backtracking from {current}")
```

### Example Usage

```python
# Clean output
print_1_to_n(5)
# Output: 1 2 3 4 5 (each on new line)

# Visualization of backtracking process
demonstrate_backtracking(3)
# Output:
# → Processing 1
#   → Processing 2
#     → Processing 3
#       Base case reached, returning...
#     ← Backtracking from 3
#   ← Backtracking from 2
# ← Backtracking from 1
```

### Key Concepts
- **Backtracking Structure:** Make choice → Explore → Backtrack
- **Forward Pass:** Print numbers in ascending order
- **Backward Pass:** Return through the recursive calls
- **Visualization:** Shows the complete recursive call stack behavior

---

## 5. Print N to 1 Using Backtracking

**Problem:** Write a recursive function that prints numbers from n down to 1 using the backtracking paradigm.

### Clean Implementation

```python
def print_n_to_1_backtrack(n, current=None):
    """
    Print numbers from n to 1 using backtracking approach.
    
    Args:
        n (int): Upper limit (starting number)
        current (int): Current number to print
    """
    # Initialize current to n if not provided
    if current is None:
        current = n
    
    # Base case: reached below 1
    if current < 1:
        return
    
    # Print current number
    print(current)
    
    # Recursive call (explore next)
    print_n_to_1_backtrack(n, current - 1)
    
    # Backtrack point (implicit return)
```

### Alternative: Using Original Parameter

```python
def print_n_to_1_simple(n):
    """
    Simpler approach using the parameter directly for backtracking.
    
    Args:
        n (int): Current number to print (decrements each call)
    """
    # Base case: reached below 1
    if n < 1:
        return
    
    # Print current number
    print(n)
    
    # Recursive call (explore next)
    print_n_to_1_simple(n - 1)
    
    # Backtrack point (implicit return)
```

### Backtracking Visualization

```python
def demonstrate_n_to_1_backtracking(n, current=None, indent=0):
    """
    Demonstrates the backtracking process for n to 1 with visual indentation.
    
    Args:
        n (int): Starting number
        current (int): Current number being processed
        indent (int): Indentation level for visualization
    """
    if current is None:
        current = n
    
    spaces = "  " * indent
    
    # Base case
    if current < 1:
        print(f"{spaces}Base case reached, returning...")
        return
    
    # Forward: make choice and explore
    print(f"{spaces}→ Processing {current}")
    demonstrate_n_to_1_backtracking(n, current - 1, indent + 1)
    
    # Backward: backtrack
    print(f"{spaces}← Backtracking from {current}")
```

### True Backtracking with Path Building

```python
def print_n_to_1_with_path(n, path=None):
    """
    True backtracking approach with explicit path tracking.
    Builds the descending sequence and then prints it.
    
    Args:
        n (int): Starting number
        path (list): Current path being built
    """
    if path is None:
        path = []
    
    # If we've built the complete sequence, print it
    if len(path) == n:
        print("Complete descending sequence:", path)
        return
    
    # Calculate next number (n, n-1, n-2, ...)
    next_num = n - len(path)
    
    # Make choice: add number to path
    path.append(next_num)
    print(f"Added {next_num} to path: {path}")
    
    # Explore: continue building the sequence
    print_n_to_1_with_path(n, path)
    
    # Backtrack: remove the number (undo the choice)
    removed = path.pop()
    print(f"Backtracked: removed {removed}, path now: {path}")
```

### Example Usage

```python
# Clean output
print_n_to_1_backtrack(5)
# Output: 5 4 3 2 1 (each on new line)

# Simple version
print_n_to_1_simple(4)
# Output: 4 3 2 1 (each on new line)

# Visualization of backtracking process
demonstrate_n_to_1_backtracking(3)
# Output:
# → Processing 3
#   → Processing 2
#     → Processing 1
#       Base case reached, returning...
#     ← Backtracking from 1
#   ← Backtracking from 2
# ← Backtracking from 3

# True backtracking with path
print_n_to_1_with_path(4)
# Shows path building and backtracking process
```

### Key Concepts
- **Countdown Pattern:** Each recursive call decrements the value
- **Backtracking Structure:** Make choice → Explore → Backtrack
- **Parameter Management:** Can use helper parameter or modify original
- **Path Building:** Explicit tracking of the sequence being constructed
- **Visualization:** Shows the complete recursive call stack behavior

### Comparison with Forward Counting

| Aspect | 1 to N | N to 1 |
|--------|---------|---------|
| **Direction** | Increment | Decrement |
| **Base Case** | `current > n` | `current < 1` |
| **Progress** | `current + 1` | `current - 1` |
| **Natural Flow** | Building up | Counting down |

---

## Summary

### Recursion Fundamentals

All recursive solutions follow the same basic structure:

1. **Base Case:** Condition to stop recursion
2. **Recursive Case:** Function calls itself with modified parameters
3. **Progress:** Each recursive call moves closer to the base case

### Key Patterns

- **Helper Parameters:** Used to track progress (current position, iteration count)
- **Call Stack Manipulation:** Printing before vs. after recursive calls affects output order
- **Backtracking:** Explicit structure showing forward exploration and backward unwinding
- **Constraint Handling:** Adding limits and boundary conditions

### Best Practices

- Always define clear base cases
- Ensure recursive calls make progress toward the base case
- Use meaningful parameter names
- Add proper documentation
- Handle edge cases (n = 0, n = 1, negative values)
- Consider space complexity due to call stack usage

### Time and Space Complexity

- **Time Complexity:** O(n) for all solutions - each number is processed once
- **Space Complexity:** O(n) due to recursive call stack depth

These problems demonstrate fundamental recursive thinking and provide a foundation for more complex recursive algorithms.
