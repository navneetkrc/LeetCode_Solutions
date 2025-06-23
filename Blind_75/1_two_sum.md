
# 🧠 Two Sum – Intuition + Optimized Solution

## 🔷 Problem Statement

> Given an array of integers `nums` and an integer `target`, return **indices** of the two numbers such that they **add up to target**.

- You **may not** use the same element twice.
- You can assume **exactly one solution exists**.
- Return the indices in **any order**.

---

## 🧪 Example

```python
nums = [2, 6, 11, 15, 7]
target = 9
````

✅ Output: `[0, 4]`
🧾 Explanation: `nums[0] + nums[4] = 2 + 7 = 9`

---

## 🚀 Optimized One-Pass HashMap Solution

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # Dictionary to store number: index
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
        return []
```

---

## 🧠 Intuitive Explanation

### 🎯 Goal:

Find two numbers `a` and `b` in the array such that:

```
a + b == target
```

Return their indices.

---

### 🔄 How the Algorithm Works (with Example)

Let’s trace through:

```python
nums = [2, 6, 11, 15, 7]
target = 9
```

| Step | Index `i` | Value `num` | Complement `= target - num` | Is Complement in `seen`? | Action          | `seen` HashMap               |
| ---- | --------- | ----------- | --------------------------- | ------------------------ | --------------- | ---------------------------- |
| 1    | 0         | 2           | 7                           | ❌ No                     | Store `2: 0`    | `{2: 0}`                     |
| 2    | 1         | 6           | 3                           | ❌ No                     | Store `6: 1`    | `{2: 0, 6: 1}`               |
| 3    | 2         | 11          | -2                          | ❌ No                     | Store `11: 2`   | `{2: 0, 6: 1, 11: 2}`        |
| 4    | 3         | 15          | -6                          | ❌ No                     | Store `15: 3`   | `{2: 0, 6: 1, 11: 2, 15: 3}` |
| 5    | 4         | 7           | 2                           | ✅ Yes (`2` at index 0)   | Return `[0, 4]` |                              |

---

### 📘 What Is `seen`?

* A hashmap (dictionary) that **stores previously seen numbers** as:

  ```python
  { number: index }
  ```

* This allows **O(1) lookup** to see if the **complement** of the current number has already been seen.

---

## 📦 Time and Space Complexity

| Complexity | Value  | Why?                             |
| ---------- | ------ | -------------------------------- |
| Time       | `O(n)` | One pass through the list        |
| Space      | `O(n)` | To store elements in the hashmap |

---

## 💬 Interview Tips

✅ **What to explain:**

* Idea of using complement: `target - num`
* Why hashmap is used (for fast lookup)
* How we avoid using the same element twice
* Time and space trade-off
* Return early on match — no need to traverse the whole array

✅ **What interviewer expects:**

* A clear understanding of hashing for lookup
* Correct index handling (not just values)
* Clean, readable code with early return

---

## 🧠 Why This Is Elegant

* One pass ✅
* No nested loops ✅
* Clean and short logic ✅
* Uses hashmap wisely for O(1) lookup ✅

---

## ✅ Final Thoughts

The **Two Sum** problem is a foundational interview question because it tests:

* Problem solving
* Hashmap usage
* Clean and optimal code

Mastering it helps build intuition for other problems involving **pair searching** and **complement logic**.

---

Great! Let's break this into two intuitive parts with simple visuals:

---

## 🅰️ A. Understanding `enumerate(nums)`

### What does `enumerate(nums)` do?

```python
for i, num in enumerate(nums):
```

This lets you loop through the list while:

* `i` → index of the current item
* `num` → actual value at that index

### ✅ Example:

```python
nums = [2, 7, 11, 15]
```

Using `enumerate(nums)`:

| Loop | `i` (Index) | `num` (Value) |
| ---- | ----------- | ------------- |
| 1    | 0           | 2             |
| 2    | 1           | 7             |
| 3    | 2           | 11            |
| 4    | 3           | 15            |

So during each loop:

* We know *where* we are (index)
* And *what* the number is

🔍 This is essential because we are **returning indices** — not just the values.

---

## 🅱️ B. Visualize How Hashmap (`seen`) is Built

The hashmap is used to **store numbers we’ve already seen** as keys, with their **indices** as values.

### 🔁 Step-by-Step Example:

Let's walk through:

```python
nums = [2, 7, 11, 15]
target = 9
```

### Iteration 1:

```python
i = 0
num = 2
complement = 9 - 2 = 7
```

* `7` is NOT in hashmap → we **store `2` with its index `0`**

**Hashmap (`seen`) after storing:**

```text
{ 2: 0 }
```

📌 This means:

> "I’ve seen number `2` at index `0`."

---

### Iteration 2:

```python
i = 1
num = 7
complement = 9 - 7 = 2
```

* `2` **IS** in hashmap!
* So, we return:

```python
[seen[2], 1] → [0, 1]
```

No need to insert `7` — we’ve already found the answer!

---

### ⛳ Summary of What Happens Each Time

| Step | `num` | `complement = target - num` | Is complement in `seen`? | Action                          | `seen` hashmap |
| ---- | ----- | --------------------------- | ------------------------ | ------------------------------- | -------------- |
| 1    | 2     | 7                           | ❌ No                     | Store 2 → `seen[2] = 0`         | `{2: 0}`       |
| 2    | 7     | 2                           | ✅ Yes                    | Return `[seen[2], 1]` → `[0,1]` |                |

---

## 💡 Why store `num: i`?

We store:

```python
seen[num] = i
```

* `num` is the number we just encountered
* `i` is its index
* So we can **quickly check later** if the number’s complement has already appeared and know **its index** to return

---

## 🧠 Mnemonic

Think of the hashmap as a **"memory of what you've seen so far."**

It answers:

* **Have I already seen a number that would complete the target sum with the current number?**
* If yes → use it
* If no → store the current number for future checks

---

Would you like this illustrated using a visual step-by-step diagram (LaTeX style or flow arrows)?

---

Perfect! Let's go step-by-step through the full code using your example:

---

## 🔢 Inputs

```python
nums = [2, 6, 11, 15, 7]
target = 9
```

We are to **find two indices** `i` and `j` such that:

```
nums[i] + nums[j] == 9
```

---

## 🧠 Code We're Using

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
        return []
```

---

## 🔍 Step-by-Step Execution

We’ll keep track of:

* Current index `i`
* Current number `num`
* Complement = `target - num`
* Hashmap `seen` after each step

---

### 🔁 Iteration 1:

```python
i = 0
num = 2
complement = 9 - 2 = 7
```

* Is 7 in `seen`? ❌ No
* Add 2 to `seen`: `{ 2: 0 }`

✅ **seen so far:**

```
{ 2: 0 }
```

---

### 🔁 Iteration 2:

```python
i = 1
num = 6
complement = 9 - 6 = 3
```

* Is 3 in `seen`? ❌ No
* Add 6 to `seen`: `{ 2: 0, 6: 1 }`

✅ **seen so far:**

```
{ 2: 0, 6: 1 }
```

---

### 🔁 Iteration 3:

```python
i = 2
num = 11
complement = 9 - 11 = -2
```

* Is -2 in `seen`? ❌ No
* Add 11 to `seen`: `{ 2: 0, 6: 1, 11: 2 }`

✅ **seen so far:**

```
{ 2: 0, 6: 1, 11: 2 }
```

---

### 🔁 Iteration 4:

```python
i = 3
num = 15
complement = 9 - 15 = -6
```

* Is -6 in `seen`? ❌ No
* Add 15 to `seen`: `{ 2: 0, 6: 1, 11: 2, 15: 3 }`

✅ **seen so far:**

```
{ 2: 0, 6: 1, 11: 2, 15: 3 }
```

---

### 🔁 Iteration 5:

```python
i = 4
num = 7
complement = 9 - 7 = 2
```

* Is 2 in `seen`? ✅ YES! Found at index `0`

### ✅ Return:

```python
[seen[2], 4] = [0, 4]
```

---

## 📦 Final Answer:

```python
[0, 4]
```

Because:

```
nums[0] = 2
nums[4] = 7
2 + 7 = 9
```

---

## 📘 Final Hashmap Trace

| Iteration | `num` | `complement` | Is Complement Found? | `seen` at this point         |
| --------- | ----- | ------------ | -------------------- | ---------------------------- |
| 0         | 2     | 7            | ❌ No                 | `{2: 0}`                     |
| 1         | 6     | 3            | ❌ No                 | `{2: 0, 6: 1}`               |
| 2         | 11    | -2           | ❌ No                 | `{2: 0, 6: 1, 11: 2}`        |
| 3         | 15    | -6           | ❌ No                 | `{2: 0, 6: 1, 11: 2, 15: 3}` |
| 4         | 7     | 2            | ✅ Yes                | return `[0, 4]`              |

---

## 🗣️ What to Explain in Interviews

1. **Complement Logic**: `target - num`
2. **Why hashmap?** Constant-time lookup to check if we’ve seen the complement before.
3. **Edge Cases**: Input guarantees exactly one solution; if not, a fallback (`return []`) is still safe.
4. **Time Complexity**: `O(n)` (single pass)
5. **Space Complexity**: `O(n)` for storing seen numbers

---
