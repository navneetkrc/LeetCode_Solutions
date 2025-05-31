
---

# 🔁 Recursive Palindrome Check in Strings

This document demonstrates how to **check if a string is a palindrome using recursion**, with step-by-step visualizations and comparison.

---

## 📘 What Is a Palindrome?

A **palindrome** is a string that reads the same forward and backward.

> Examples:
> ✅ "madam", "racecar", "a", "abba"
> ❌ "hello", "world", "python"

---

## ✅ 1. Recursive Palindrome Check Using Two Pointers

### **Concept**

Use two pointers: `left` (start of the string) and `right` (end of the string). Compare the characters:

* If they match, move both inward: `left+1`, `right-1`
* If mismatch: return `False`
* If `left >= right`, it's a palindrome

### **Python Code**

```python
def is_palindrome_two_pointers(s, left, right):
    if left >= right:
        return True
    if s[left] != s[right]:
        return False
    return is_palindrome_two_pointers(s, left + 1, right - 1)
```

### **Example**

```python
s = "racecar"
print(is_palindrome_two_pointers(s, 0, len(s) - 1))  # Output: True
```

### **Step-by-Step Visualization**

| Call | Check                    | Result  | Next Call     |
| ---- | ------------------------ | ------- | ------------- |
| 1    | s\[0] = 'r', s\[6] = 'r' | ✅ match | (1, 5)        |
| 2    | s\[1] = 'a', s\[5] = 'a' | ✅ match | (2, 4)        |
| 3    | s\[2] = 'c', s\[4] = 'c' | ✅ match | (3, 3)        |
| 4    | left >= right            | ✅ done  | return `True` |

---

## ✅ 2. Recursive Palindrome Check Using String Slicing

### **Concept**

Compare the first and last characters:

* If they match, recurse on the **substring** `s[1:-1]`
* If mismatch, return `False`
* If string length ≤ 1, it's a palindrome

### **Python Code**

```python
def is_palindrome_slicing(s):
    if len(s) <= 1:
        return True
    if s[0] != s[-1]:
        return False
    return is_palindrome_slicing(s[1:-1])
```

### **Example**

```python
s = "level"
print(is_palindrome_slicing(s))  # Output: True
```

### **Step-by-Step Visualization**

| Call | String  | First vs Last | Result  | Next Call   |
| ---- | ------- | ------------- | ------- | ----------- |
| 1    | "level" | 'l' vs 'l'    | ✅ match | "eve"       |
| 2    | "eve"   | 'e' vs 'e'    | ✅ match | "v"         |
| 3    | "v"     | Length 1      | ✅ done  | return True |

---

## 🧠 Comparison Table

| Feature                 | Two Pointers                      | String Slicing                      |
| ----------------------- | --------------------------------- | ----------------------------------- |
| Parameters              | Requires passing `left`, `right`  | Only the string                     |
| Clarity                 | Explicit character position check | More elegant / clean                |
| Performance             | Faster (no new string creation)   | Slower due to slicing (O(n²))       |
| Memory                  | O(n) stack frames                 | O(n) stack + copies of substrings   |
| Tail-Recursion Friendly | ✅ (last action is recursion)      | ❌ (substring creation after return) |
| Ideal For               | Performance-conscious code        | Cleaner code, small inputs          |

---

## 📝 Conclusion

* **Use Two-Pointer Approach**:

  * ✅ Better performance
  * ✅ No unnecessary memory overhead
  * ✅ Ideal for large strings

* **Use Slicing Approach**:

  * ✅ Clean, minimal code
  * ✅ Suitable for short strings or learning

---

## 📌 Real-World Tip

Python does **not** optimize tail calls, so **both methods can hit recursion limits** on long strings. For production code, consider using an **iterative approach**.

---
![image](https://github.com/user-attachments/assets/57b558b2-a59c-49b2-9fe5-858fe1d06a08)


---
Claude 4 
---
# Recursive Palindrome Checking - Complete Guide

A palindrome is a string that reads the same forwards and backwards. In this guide, we'll explore multiple recursive approaches to check if a string is a palindrome, with detailed visualizations and comparisons.

## Approach 1: Two-Pointer Recursive Method

This approach uses two indices (start and end) that move towards each other.

```python
def is_palindrome_two_pointer(s, start=0, end=None):
    """
    Check if string is palindrome using two pointers recursively
    """
    if end is None:
        end = len(s) - 1
    
    # Base case: pointers meet or cross
    if start >= end:
        return True
    
    # If characters don't match
    if s[start] != s[end]:
        return False
    
    # Recursive call with moved pointers
    return is_palindrome_two_pointer(s, start + 1, end - 1)
```

### Visualization for "racecar":
```
Step 1: s[0] vs s[6] → 'r' == 'r' ✓
        r a c e c a r
        ↑           ↑
        start       end

Step 2: s[1] vs s[5] → 'a' == 'a' ✓
        r a c e c a r
          ↑       ↑
          start   end

Step 3: s[2] vs s[4] → 'c' == 'c' ✓
        r a c e c a r
            ↑   ↑
            start end

Step 4: start(3) >= end(3) → Base case reached → True
```

### Test Results:
```python
# Test cases
test_strings = ["racecar", "hello", "madam", "a", ""]

for s in test_strings:
    result = is_palindrome_two_pointer(s)
    print(f"'{s}' is palindrome: {result}")

# Output:
# 'racecar' is palindrome: True
# 'hello' is palindrome: False
# 'madam' is palindrome: True
# 'a' is palindrome: True
# '' is palindrome: True
```

---

## Approach 2: Single Index Method

This approach uses only one index and compares with the corresponding position from the end.

```python
def is_palindrome_single_index(s, index=0):
    """
    Check if string is palindrome using single index recursively
    """
    # Base case: reached middle of string
    if index >= len(s) // 2:
        return True
    
    # Compare character at index with its mirror from end
    if s[index] != s[len(s) - 1 - index]:
        return False
    
    # Recursive call with next index
    return is_palindrome_single_index(s, index + 1)
```

### Visualization for "level":
```
Step 1: s[0] vs s[4] → 'l' == 'l' ✓
        l e v e l
        ↑       ↑
        index   mirror

Step 2: s[1] vs s[3] → 'e' == 'e' ✓
        l e v e l
          ↑   ↑
          index mirror

Step 3: index(2) >= len(5)//2 → Base case reached → True
        l e v e l
            ↑
            middle
```

---

## Approach 3: String Slicing Method

This approach compares the first and last characters, then recursively checks the substring.

```python
def is_palindrome_slicing(s):
    """
    Check if string is palindrome using string slicing recursively
    """
    # Base cases
    if len(s) <= 1:
        return True
    
    # Compare first and last characters
    if s[0] != s[-1]:
        return False
    
    # Recursive call with middle substring
    return is_palindrome_slicing(s[1:-1])
```

### Visualization for "civic":
```
Step 1: 'civic' → 'c' == 'c' ✓, check 'ivi'
        c i v i c
        ↑       ↑

Step 2: 'ivi' → 'i' == 'i' ✓, check 'v'
        i v i
        ↑   ↑

Step 3: 'v' → len <= 1 → True
        v
        ↑
```

---

## Approach 4: Divide and Conquer Method

This approach splits the string in half and compares the first half with the reverse of the second half.

```python
def is_palindrome_divide_conquer(s):
    """
    Check if string is palindrome using divide and conquer recursively
    """
    # Base case
    if len(s) <= 1:
        return True
    
    mid = len(s) // 2
    
    if len(s) % 2 == 0:
        # Even length: compare first half with reverse of second half
        left_half = s[:mid]
        right_half = s[mid:]
    else:
        # Odd length: ignore middle character
        left_half = s[:mid]
        right_half = s[mid + 1:]
    
    # Check if left half equals reverse of right half
    return left_half == reverse_string_recursive(right_half)

def reverse_string_recursive(s):
    """Helper function to reverse string recursively"""
    if len(s) <= 1:
        return s
    return s[-1] + reverse_string_recursive(s[:-1])
```

### Visualization for "abcba":
```
Original: "abcba" (odd length)
          a b c b a
          ├─┤ ┌─┤ ├─┤
          left │  right
               middle

Step 1: Split → left="ab", middle='c', right="ba"
Step 2: Reverse right → "ab"
Step 3: Compare left("ab") == reversed_right("ab") → True
```

---

## Performance Comparison

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|----------------|------------------|------|------|
| **Two-Pointer** | O(n) | O(n) | • Clear logic<br>• Efficient comparisons | • Two parameters needed |
| **Single Index** | O(n) | O(n) | • Only one index variable<br>• Clean implementation | • Slightly more calculation |
| **String Slicing** | O(n²) | O(n²) | • Very intuitive<br>• Minimal code | • Creates new strings<br>• Higher memory usage |
| **Divide & Conquer** | O(n²) | O(n²) | • Demonstrates D&C concept<br>• Educational value | • Most complex<br>• Inefficient for this problem |

## Memory Usage Analysis

### Stack Depth Comparison for "racecar":
```
Two-Pointer:     4 recursive calls
Single Index:    3 recursive calls  
String Slicing:  4 recursive calls (+ string creation overhead)
Divide & Conquer: 4 recursive calls (+ helper function calls)
```

## Recommendations

1. **For Production Code**: Use **Two-Pointer** or **Single Index** approach
   - Both are O(n) time and space efficient
   - Single Index is slightly cleaner with one parameter

2. **For Learning Recursion**: Try **String Slicing** approach first
   - Most intuitive to understand
   - Clearly shows recursive structure

3. **For Algorithm Education**: **Divide & Conquer** demonstrates the concept
   - Good for understanding the paradigm
   - Not optimal for palindrome checking

## Complete Test Suite

```python
def test_all_approaches():
    test_cases = [
        ("", True),           # Empty string
        ("a", True),          # Single character
        ("aa", True),         # Two same characters
        ("ab", False),        # Two different characters
        ("racecar", True),    # Odd length palindrome
        ("level", True),      # Even length palindrome
        ("hello", False),     # Not a palindrome
        ("A man a plan a canal Panama", False),  # With spaces
        ("Aa", False),        # Case sensitive
    ]
    
    approaches = [
        ("Two-Pointer", is_palindrome_two_pointer),
        ("Single Index", is_palindrome_single_index),
        ("String Slicing", is_palindrome_slicing),
        ("Divide & Conquer", is_palindrome_divide_conquer)
    ]
    
    for approach_name, func in approaches:
        print(f"\n{approach_name} Results:")
        print("-" * 30)
        for test_string, expected in test_cases:
            result = func(test_string)
            status = "✓" if result == expected else "✗"
            print(f"{status} '{test_string}' → {result}")
```

## Conclusion

Each recursive approach has its merits. The **Single Index** method offers the best balance of simplicity and efficiency, while the **Two-Pointer** method provides clear algorithmic thinking. Choose based on your specific needs: performance, readability, or educational value.
