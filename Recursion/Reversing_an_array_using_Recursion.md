
---

# Recursive Array Reversal Techniques

This document demonstrates two recursive methods for reversing an array:

1. **Using Two Pointers** (start and end)
2. **Using a Single Variable** (index-based)

Each method is visualized with step-by-step tracing and compared for its advantages.

---

## 🔁 1. Reversing Array Using Two Pointers

### **Concept**

Use two pointers: one starting from the beginning (`left`), the other from the end (`right`). Swap elements at those positions and recursively move the pointers toward the center.

### **Python Code**

```python
def reverse_two_pointers(arr, left, right):
    if left >= right:
        return
    arr[left], arr[right] = arr[right], arr[left]
    reverse_two_pointers(arr, left + 1, right - 1)
```

### **Example**

```python
arr = [1, 2, 3, 4, 5]
reverse_two_pointers(arr, 0, len(arr) - 1)
print(arr)  # [5, 4, 3, 2, 1]
```

### **Step-by-Step Visualization**

| Call | Array State       | Swap                           |
| ---- | ----------------- | ------------------------------ |
| 1    | `[1, 2, 3, 4, 5]` | Swap(0, 4) → `[5, 2, 3, 4, 1]` |
| 2    | `[5, 2, 3, 4, 1]` | Swap(1, 3) → `[5, 4, 3, 2, 1]` |
| 3    | `[5, 4, 3, 2, 1]` | Pointers meet at index 2       |

---

## 🔁 2. Reversing Array Using a Single Variable (Index)

### **Concept**

Use one index that increases recursively. For each recursive call, swap the element at index `i` with its mirror element from the end: `n - 1 - i`.

### **Python Code**

```python
def reverse_single_index(arr, i=0):
    n = len(arr)
    if i >= n // 2:
        return
    arr[i], arr[n - 1 - i] = arr[n - 1 - i], arr[i]
    reverse_single_index(arr, i + 1)
```

### **Example**

```python
arr = [1, 2, 3, 4, 5]
reverse_single_index(arr)
print(arr)  # [5, 4, 3, 2, 1]
```

### **Step-by-Step Visualization**

| Call | Index `i` | Swap                           | Array State |
| ---- | --------- | ------------------------------ | ----------- |
| 1    | 0         | Swap(0, 4) → `[5, 2, 3, 4, 1]` |             |
| 2    | 1         | Swap(1, 3) → `[5, 4, 3, 2, 1]` |             |
| 3    | 2         | `i >= n//2`, recursion ends    |             |

---

## 📊 Comparison Table

| Feature                 | Two Pointers                | Single Variable (Index)            |
| ----------------------- | --------------------------- | ---------------------------------- |
| Variables Used          | 2 (`left`, `right`)         | 1 (`i`)                            |
| Clarity                 | High — clearly shows bounds | Slightly abstract                  |
| Memory                  | O(n) due to recursion       | O(n) due to recursion              |
| Reusability (in-place)  | ✅                           | ✅                                  |
| Tail-Recursion Friendly | ✅ (can be optimized)        | ✅                                  |
| Symmetry Awareness      | Explicit                    | Implicit (via `n - 1 - i`)         |
| Ideal For               | Beginners, clarity          | Simplicity, less parameter passing |

---

## 🏁 Final Thoughts

* **Use Two Pointers** when you want clearer control over both ends of the array.
* **Use One Index** when you want cleaner function signatures or are dealing with symmetric operations.
* Both approaches demonstrate **recursive in-place reversal**, with minor differences in readability and parameter complexity.

> Note: In practice, both are functionally equivalent. Python does not optimize tail calls, so recursion depth matters for large arrays.

---

Let me know if you'd like a visual animation or a diagram to illustrate each step interactively!
