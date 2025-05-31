
---

# 🌀 Recursive Fibonacci Approaches: Explanation & Comparison

## 📌 Problem Statement

> **Compute the nth Fibonacci number using recursion.**
> The Fibonacci sequence is:
> `0, 1, 1, 2, 3, 5, 8, 13, ...`
> With the base cases:
>
> * `fib(0) = 0`
> * `fib(1) = 1`
> * `fib(n) = fib(n-1) + fib(n-2)` for n > 1

---

## 🔁 1. Naive Recursive Fibonacci

### ✅ Code

```python
def fibonacci_naive(n):
    if n <= 1:
        return n
    return fibonacci_naive(n - 1) + fibonacci_naive(n - 2)
```

### 🔍 Visualization (n = 5)

```
                    fib(5)
                  /        \
             fib(4)         fib(3)
            /     \         /     \
       fib(3)   fib(2)  fib(2)  fib(1)
       /    \     / \     / \
   fib(2) fib(1) fib(1) fib(0)
   / \
fib(1) fib(0)
```

### ⏱️ Time Complexity: `O(2^n)`

### ❌ Disadvantages:

* Recomputes values repeatedly.
* Exponential time complexity.
* Poor performance for large `n`.

---

![image](https://github.com/user-attachments/assets/76d6031e-f947-4a04-9f4d-2b5040bcd27b)

---
## 🧠 2. Memoized Recursive Fibonacci

### ✅ Code

```python
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]
```

### 🔍 Visualization (n = 5)

```
fib(5)
↳ fib(4)
   ↳ fib(3)
      ↳ fib(2)
         ↳ fib(1) → 1
         ↳ fib(0) → 0
      → fib(2) = 1
   → fib(3) = 2
↳ fib(3) → already in memo → 2
→ fib(5) = 5
```

### ⏱️ Time Complexity: `O(n)`

### ✅ Advantages:

* Efficient and fast.
* No repeated computations.
* Suitable for large `n`.

---

## 🔄 3. Tail Recursive Fibonacci

### ✅ Code

```python
def fibonacci_tail(n, a=0, b=1):
    if n == 0:
        return a
    return fibonacci_tail(n-1, b, a + b)
```

### 🔍 Visualization (n = 5)

```
fibonacci_tail(5, 0, 1)
→ fibonacci_tail(4, 1, 1)
→ fibonacci_tail(3, 1, 2)
→ fibonacci_tail(2, 2, 3)
→ fibonacci_tail(1, 3, 5)
→ fibonacci_tail(0, 5, 8)
→ return 5
```

### ⏱️ Time Complexity: `O(n)`

### ✅ Advantages:

* Constant stack space (if language supports TCO).
* No repeated calculations.
* More memory-efficient than naive.

---

## 📊 Comparison Table

| Approach           | Time Complexity | Space Complexity | Pros                       | Cons                              |
| ------------------ | --------------- | ---------------- | -------------------------- | --------------------------------- |
| Naive Recursion    | O(2^n)          | O(n)             | Simple to implement        | Exponential time, redundant calls |
| Memoized Recursion | O(n)            | O(n)             | Fast, avoids recomputation | Uses extra memory for memoization |
| Tail Recursion     | O(n)            | O(n) in Python   | Efficient in TCO languages | Python doesn't support TCO        |

---

## ✅ Summary

* Use **memoized recursion** for readable and efficient code in Python.
* Use **tail recursion** where tail call optimization is supported (e.g., in Scala or Scheme).
* Avoid **naive recursion** for large `n` unless for teaching or small inputs.

---

# Recursive Fibonacci - Complete Guide

The Fibonacci sequence is a series where each number is the sum of the two preceding ones: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...

In this guide, we'll explore multiple recursive approaches to calculate the nth Fibonacci number, with detailed visualizations and comparisons.

## Approach 1: Basic Recursive Method (Naive)

This is the most straightforward translation of the mathematical definition.

```python
def fibonacci_basic(n):
    """
    Basic recursive Fibonacci implementation
    F(n) = F(n-1) + F(n-2)
    """
    # Base cases
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    
    # Recursive case
    return fibonacci_basic(n - 1) + fibonacci_basic(n - 2)
```

### Visualization for fibonacci_basic(5):
```
                    fib(5)
                   /      \
               fib(4)      fib(3)
              /     \      /     \
          fib(3)   fib(2) fib(2) fib(1)
         /    \    /   \   /   \    |
     fib(2) fib(1) fib(1) fib(0) fib(1) fib(0)  1
     /   \    |      |      |      |      |
  fib(1) fib(0) 1    1      0      1      0
    |      |
    1      0

Call count: 15 function calls
Result: 5
```

### Test Results:
```python
# Test basic recursive approach
for i in range(10):
    result = fibonacci_basic(i)
    print(f"F({i}) = {result}")

# Output:
# F(0) = 0    F(5) = 5
# F(1) = 1    F(6) = 8
# F(2) = 1    F(7) = 13
# F(3) = 2    F(8) = 21
# F(4) = 3    F(9) = 34
```

---

## Approach 2: Memoized Recursive Method

This approach stores previously calculated values to avoid redundant calculations.

```python
def fibonacci_memoized(n, memo=None):
    """
    Memoized recursive Fibonacci implementation
    Stores computed values to avoid recalculation
    """
    if memo is None:
        memo = {}
    
    # Base cases
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    
    # Check if already computed
    if n in memo:
        return memo[n]
    
    # Compute and store in memo
    memo[n] = fibonacci_memoized(n - 1, memo) + fibonacci_memoized(n - 2, memo)
    return memo[n]
```

### Visualization for fibonacci_memoized(5):
```
Step 1: fib(5) → not in memo
        ├── fib(4) → not in memo
        │   ├── fib(3) → not in memo
        │   │   ├── fib(2) → not in memo
        │   │   │   ├── fib(1) → return 1
        │   │   │   └── fib(0) → return 0
        │   │   │   memo[2] = 1
        │   │   └── fib(1) → return 1
        │   │   memo[3] = 2
        │   └── fib(2) → found in memo → return 1
        │   memo[4] = 3
        └── fib(3) → found in memo → return 2
        memo[5] = 5

Call count: 9 function calls (vs 15 in basic)
Memo final state: {2: 1, 3: 2, 4: 3, 5: 5}
```

---

## Approach 3: Tail Recursive Method

This approach uses tail recursion with accumulator parameters.

```python
def fibonacci_tail_recursive(n, a=0, b=1, count=0):
    """
    Tail recursive Fibonacci implementation
    Uses accumulator pattern for optimization
    """
    # Base case: reached target count
    if count == n:
        return a
    
    # Tail recursive call
    return fibonacci_tail_recursive(n, b, a + b, count + 1)

# Wrapper function for cleaner interface
def fibonacci_tail(n):
    if n <= 0:
        return 0
    return fibonacci_tail_recursive(n)
```

### Visualization for fibonacci_tail(5):
```
Call 1: fibonacci_tail_recursive(5, a=0, b=1, count=0)
        count(0) != n(5), continue
        
Call 2: fibonacci_tail_recursive(5, a=1, b=1, count=1)
        count(1) != n(5), continue
        
Call 3: fibonacci_tail_recursive(5, a=1, b=2, count=2)
        count(2) != n(5), continue
        
Call 4: fibonacci_tail_recursive(5, a=2, b=3, count=3)
        count(3) != n(5), continue
        
Call 5: fibonacci_tail_recursive(5, a=3, b=5, count=4)
        count(4) != n(5), continue
        
Call 6: fibonacci_tail_recursive(5, a=5, b=8, count=5)
        count(5) == n(5), return a = 5

Linear progression: 0 → 1 → 1 → 2 → 3 → 5
Call count: 6 function calls
```

---

## Approach 4: Helper Method Recursive

This approach separates the recursive logic into a helper function for better organization.

```python
def fibonacci_helper_method(n):
    """
    Fibonacci using helper method pattern
    """
    def fib_helper(current, target, prev=0, curr=1):
        # Base case
        if current == target:
            return prev
        
        # Recursive case
        return fib_helper(current + 1, target, curr, prev + curr)
    
    if n <= 0:
        return 0
    
    return fib_helper(0, n)
```

### Visualization for fibonacci_helper_method(6):
```
Initial call: fibonacci_helper_method(6)
├── Base check: 6 > 0, call helper
└── fib_helper(0, 6, prev=0, curr=1)

Helper calls progression:
Call 1: fib_helper(0, 6, prev=0, curr=1) → 0 != 6
        Next: fib_helper(1, 6, prev=1, curr=0+1=1)

Call 2: fib_helper(1, 6, prev=1, curr=1) → 1 != 6
        Next: fib_helper(2, 6, prev=1, curr=1+1=2)

Call 3: fib_helper(2, 6, prev=1, curr=2) → 2 != 6
        Next: fib_helper(3, 6, prev=2, curr=1+2=3)

Call 4: fib_helper(3, 6, prev=2, curr=3) → 3 != 6
        Next: fib_helper(4, 6, prev=3, curr=2+3=5)

Call 5: fib_helper(4, 6, prev=3, curr=5) → 4 != 6
        Next: fib_helper(5, 6, prev=5, curr=3+5=8)

Call 6: fib_helper(5, 6, prev=5, curr=8) → 5 != 6
        Next: fib_helper(6, 6, prev=8, curr=5+8=13)

Call 7: fib_helper(6, 6, prev=8, curr=13) → 6 == 6
        Return prev = 8 ✓

Sequence built: 0 → 1 → 1 → 2 → 3 → 5 → 8
Result: F(6) = 8
```

---

## Approach 5: Matrix Recursion Method

This approach uses matrix exponentiation for mathematical elegance.

```python
def fibonacci_matrix(n):
    """
    Fibonacci using matrix exponentiation recursively
    [[F(n+1), F(n)  ], = [[1, 1],^n
     [F(n),   F(n-1)]]    [1, 0]]
    """
    def matrix_multiply(A, B):
        """Multiply two 2x2 matrices"""
        return [
            [A[0][0]*B[0][0] + A[0][1]*B[1][0], A[0][0]*B[0][1] + A[0][1]*B[1][1]],
            [A[1][0]*B[0][0] + A[1][1]*B[1][0], A[1][0]*B[0][1] + A[1][1]*B[1][1]]
        ]
    
    def matrix_power(matrix, power):
        """Recursively compute matrix^power"""
        if power == 0:
            return [[1, 0], [0, 1]]  # Identity matrix
        elif power == 1:
            return matrix
        elif power % 2 == 0:
            half_power = matrix_power(matrix, power // 2)
            return matrix_multiply(half_power, half_power)
        else:
            return matrix_multiply(matrix, matrix_power(matrix, power - 1))
    
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    
    base_matrix = [[1, 1], [1, 0]]
    result_matrix = matrix_power(base_matrix, n - 1)
    return result_matrix[0][0]
```

### Visualization for fibonacci_matrix(5):
```
Base matrix: [[1, 1],    Target: matrix^4
             [1, 0]]

Step 1: matrix_power([[1,1],[1,0]], 4)
        4 is even, so: half = matrix_power(matrix, 2)
        
Step 2: matrix_power([[1,1],[1,0]], 2)  
        2 is even, so: half = matrix_power(matrix, 1)
        
Step 3: matrix_power([[1,1],[1,0]], 1)
        Return [[1,1],[1,0]]
        
Step 4: Back to step 2, multiply half × half:
        [[1,1],[1,0]]² = [[2,1],[1,1]]
        
Step 5: Back to step 1, multiply result × result:
        [[2,1],[1,1]]² = [[5,3],[3,2]]

Final matrix: [[5,3],[3,2]]
Result: matrix[0][0] = 5
```

---

## Performance Comparison

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|----------------|------------------|------|------|
| **Basic Recursive** | O(2ⁿ) | O(n) | • Simple to understand<br>• Direct mathematical translation | • Exponential time<br>• Many redundant calculations |
| **Memoized** | O(n) | O(n) | • Linear time complexity<br>• Eliminates redundancy | • Extra memory for memo<br>• Still recursive overhead |
| **Tail Recursive** | O(n) | O(n)* | • Efficient for languages with TCO<br>• Constant space potential | • Python doesn't optimize TCO<br>• Less intuitive |
| **Helper Method** | O(n) | O(n) | • Clean separation of concerns<br>• Good organization | • Similar to tail recursive<br>• Extra function call overhead |
| **Matrix Method** | O(log n) | O(log n) | • Logarithmic complexity<br>• Mathematically elegant | • Complex implementation<br>• Overkill for small n |

*In languages with Tail Call Optimization

## Call Count Analysis

### For fibonacci(10):
```
Basic Recursive:    177 calls
Memoized:          19 calls  
Tail Recursive:    11 calls
Helper Method:     11 calls
Matrix Method:     ~4 calls (log₂(10))
```

### Growth Pattern:
```python
def count_calls():
    call_counts = []
    
    for n in range(1, 8):
        # Reset call counter
        global call_count
        call_count = 0
        
        # Test basic recursive
        def fib_counting(n):
            global call_count
            call_count += 1
            if n <= 1:
                return n
            return fib_counting(n-1) + fib_counting(n-2)
        
        fib_counting(n)
        call_counts.append((n, call_count))
    
    return call_counts

# Results show exponential growth:
# F(1): 1 call    F(5): 15 calls
# F(2): 3 calls   F(6): 25 calls  
# F(3): 5 calls   F(7): 41 calls
# F(4): 9 calls   Pattern: ~φⁿ calls
```

## Memory Usage Visualization

### Stack Depth Comparison for F(6):
```
Basic Recursive:     6 levels deep (worst case)
Memoized:           6 levels deep (first calculation)
Tail Recursive:     6 levels deep (no TCO in Python)
Helper Method:      6 levels deep
Matrix Method:      3 levels deep (log₂(6))

Stack visualization for Basic F(4):
Level 1: fib(4)
Level 2: ├─ fib(3), fib(2)
Level 3: │  ├─ fib(2), fib(1)  │ ├─ fib(1), fib(0)
Level 4: │  │  ├─ fib(1), fib(0)
Level 5: │  │  │  (returns)
```

## Practical Recommendations

### For Different Use Cases:

1. **Learning Recursion**: Start with **Basic Recursive**
   ```python
   # Easy to understand, shows pure recursive thinking
   def fib(n):
       if n <= 1: return n
       return fib(n-1) + fib(n-2)
   ```

2. **Production Code**: Use **Memoized Recursive**
   ```python
   # Best balance of clarity and performance
   from functools import lru_cache
   
   @lru_cache(maxsize=None)
   def fib(n):
       if n <= 1: return n
       return fib(n-1) + fib(n-2)
   ```

3. **Large Numbers**: Use **Matrix Method**
   ```python
   # For fibonacci(1000) and beyond
   # Logarithmic complexity shines here
   ```

4. **Educational/Interview**: **Tail Recursive** shows advanced understanding
   ```python
   # Demonstrates knowledge of optimization techniques
   def fib(n, a=0, b=1):
       return a if n == 0 else fib(n-1, b, a+b)
   ```

## Complete Test Suite

```python
import time
from functools import lru_cache

def performance_test():
    """Compare performance of all approaches"""
    
    test_values = [10, 20, 30]
    approaches = [
        ("Basic", fibonacci_basic),
        ("Memoized", fibonacci_memoized),
        ("Tail", fibonacci_tail),
        ("Helper", fibonacci_helper_method),
        ("Matrix", fibonacci_matrix)
    ]
    
    print("Performance Comparison:")
    print("=" * 50)
    
    for n in test_values:
        print(f"\nFibonacci({n}):")
        print("-" * 20)
        
        for name, func in approaches:
            if name == "Basic" and n > 25:
                print(f"{name:10}: Skipped (too slow)")
                continue
                
            start_time = time.time()
            result = func(n)
            end_time = time.time()
            
            print(f"{name:10}: {result} ({end_time-start_time:.6f}s)")

def correctness_test():
    """Verify all approaches give same results"""
    known_fibonacci = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
    
    approaches = [
        fibonacci_basic,
        fibonacci_memoized, 
        fibonacci_tail,
        fibonacci_helper_method,
        fibonacci_matrix
    ]
    
    print("Correctness Test:")
    print("=" * 30)
    
    for i, expected in enumerate(known_fibonacci):
        results = [func(i) for func in approaches]
        all_correct = all(r == expected for r in results)
        status = "✓" if all_correct else "✗"
        print(f"{status} F({i}) = {expected} → {results}")
```

## Mathematical Insights

### Golden Ratio Connection:
The Fibonacci sequence has a fascinating connection to the golden ratio (φ ≈ 1.618):

```
F(n) = (φⁿ - ψⁿ) / √5

where:
φ = (1 + √5) / 2 ≈ 1.618034 (golden ratio)
ψ = (1 - √5) / 2 ≈ -0.618034

This explains why basic recursion is O(φⁿ) ≈ O(1.618ⁿ)
```

### Matrix Formula Explanation:
```
[[1, 1],^n = [[F(n+1), F(n)  ],
 [1, 0]]      [F(n),   F(n-1)]]

This allows us to compute F(n) in O(log n) time using
fast matrix exponentiation!
```

## Conclusion

Each recursive approach offers different trade-offs:

- **Basic Recursive**: Perfect for learning, terrible for performance
- **Memoized**: Best general-purpose solution 
- **Tail Recursive**: Shows optimization awareness
- **Matrix Method**: Optimal for very large numbers

Choose based on your specific needs: educational value, performance requirements, or code clarity. The memoized approach typically offers the best balance for most practical applications.

