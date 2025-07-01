# 🔥 Stack Interview Prep Guide (Advanced Stack Patterns)

---

## 🔷 Pattern: **Monotonic Stack – Pattern-Specific Mastery**

---

### 🟩 Q12. **Remove K Digits**

* 📌 **Problem**: Given a number represented as a string and an integer `k`, remove `k` digits to form the **smallest number possible**.

---

### 🧠 Interview Intent

* Test understanding of **monotonic increasing stack**
* Simulates greedy decision-making with stack discipline
* Checks for ability to **preserve order** and minimize value

---

### 📝 Step-by-Step Walkthrough

**Input**: `"1432219", k=3`
**Goal**: Smallest number after removing 3 digits

**Stack Simulation**:

```
char | Stack            | k | Comment
-----|------------------|---|-------------------------------
1    | [1]              | 3 | Add 1
4    | [1, 4]           | 3 | Add 4
3    | [1]              | 2 | Pop 4 (4 > 3)
      [1, 3]
2    | [1]              | 1 | Pop 3 (3 > 2)
      [1, 2]
2    | [1, 2, 2]        | 1 | Add 2
1    | [1, 2]           | 0 | Pop 2 (2 > 1), then done
      [1, 2, 1]
9    | [1, 2, 1, 9]     | 0 | Final add
```

Final: `"1219"` after removing leading zeros

---

### ⚠️ Common Pitfalls

* Forgetting to handle **leading zeroes**
* Not checking `while stack and k > 0 and stack[-1] > c` correctly
* Not trimming leftover digits if `k > 0` after traversal

---

### 🔄 Follow-up Variations

* Remove **K digits to form largest number**
* Given a number, remove digits so it's divisible by X

---

### 💡 Optimization Hints

* Time: O(n), Single pass with stack
* Space: O(n) stack

---

### ✅ Template Code

```python
def removeKdigits(num: str, k: int) -> str:
    stack = []
    for digit in num:
        while stack and k > 0 and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)

    # Remove leftover digits from end
    final_stack = stack[:-k] if k else stack
    result = ''.join(final_stack).lstrip('0')
    return result if result else "0"
```

---

### 🔍 Real-world Analogy

Think of it as building the **smallest number with allowed removals** — like crafting a minimal password by removing insecure digits.

---

### 🚨 Interview Red Flags

* Using sort-based greedy (destroys original order)
* Brute force with substrings (O(n^k) solutions)

---

### 🎯 Talking Points

* “Since order matters, I’m using a monotonic stack to greedily remove large digits from left to right.”
* “Leading zeros are handled post-processing.”

---

---

### 🟧 Q13. **132 Pattern Detection**

* 📌 **Problem**: Given an array, find whether there exists a subsequence `ai, aj, ak` such that `i < j < k` and `ai < ak < aj`

---

### 🧠 Interview Intent

* Tests ability to **reverse-engineer patterns**
* Use of **reverse traversal + monotonic decreasing stack**
* Insight into when to use stack from **right → left**

---

### 📝 Step-by-Step Walkthrough

**Input**: `[3, 1, 4, 2]`
**Target Pattern**: Find if any triplet exists in `ai < ak < aj`

**Logic**:

* Traverse from **right to left**
* Maintain stack of `aj` candidates (monotonically decreasing)
* Track `ak` (the middle value)

```
Stack = [] | s3 = -inf

i = 3 → 2 → stack empty → push 2 → s3 = -inf
i = 2 → 4 > stack[-1] → pop 2 → s3 = 2 → push 4
i = 1 → 1 < s3 (1 < 2) → FOUND ai < ak < aj
```

---

### ⚠️ Common Pitfalls

* Not realizing that the **middle element** `ak` is tracked separately
* Traversing from **left to right** (wrong direction!)

---

### 🔄 Follow-up Variants

* Find the longest such subsequence
* Count total number of such patterns

---

### 💡 Optimization Hints

* Time: O(n), reverse stack pass
* Track `ak` (s3) to validate `ai < ak < aj`

---

### ✅ Template Code

```python
def find132pattern(nums):
    stack = []
    s3 = float('-inf')
    for num in reversed(nums):
        if num < s3:
            return True
        while stack and num > stack[-1]:
            s3 = stack.pop()
        stack.append(num)
    return False
```

---

### 🔍 Real-world Analogy

Imagine stock prices where you buy low (ai), expect it to rise (aj), but then it dips slightly (ak) — this pattern often implies **risk or opportunity**.

---

### 🚨 Interview Red Flags

* Using 3 nested loops
* Not tracking the `ak` value or interpreting stack incorrectly

---

### 🎯 Talking Points

* “This is a classic reverse monotonic pattern recognition problem.”
* “I track the third value `ak` explicitly to validate against left values.”

---

---

### 🟥 Q14. **Maximum Subarray Min-Product**

* 📌 **Problem**: Given an array, return the **maximum of (min(subarray) \* sum(subarray))**

---

### 🧠 Interview Intent

* Tests **monotonic increasing stack** usage for span limits
* Requires prefix sums + combining subarray range with constraints

---

### 📝 Step-by-Step Walkthrough

1. Build prefix sum
2. For each element `nums[i]`, consider it the **minimum** of a subarray
3. Use stack to find:

   * NSER (right boundary)
   * NSEL (left boundary)
4. Compute sum for subarray → multiply with `nums[i]`

**Example**: `nums = [3, 1, 5, 6, 4, 2]`

Prefix sum: `[0, 3, 4, 9, 15, 19, 21]`

Calculate for each index using:

```
left[i] = index of prev smaller (NSEL)
right[i] = index of next smaller (NSER)
sum = prefix[right[i]] - prefix[left[i]+1]
res = max(res, sum * nums[i])
```

---

### ⚠️ Common Pitfalls

* Forgetting that NSER/NSEL should work with **indices**
* Incorrect prefix sum boundaries
* Confusing min-product with max-subarray sum (Kadane)

---

### 🔄 Follow-up Variants

* Max rectangle area in histogram (very related)
* Max sum-subarray with constraints

---

### 💡 Optimization Hints

* Time: O(n), single pass stack for NSEL/NSER
* Space: O(n) for prefix, stack

---

### ✅ Template Code

```python
def maxSumMinProduct(nums):
    n = len(nums)
    prefix = [0] * (n+1)
    for i in range(n):
        prefix[i+1] = prefix[i] + nums[i]

    stack, left, right = [], [0]*n, [n]*n

    for i in range(n):
        while stack and nums[stack[-1]] >= nums[i]:
            right[stack[-1]] = i
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    max_prod = 0
    for i in range(n):
        total = prefix[right[i]] - prefix[left[i]+1]
        max_prod = max(max_prod, total * nums[i])
    return max_prod % (10**9 + 7)
```

---

### 🔍 Real-world Analogy

Imagine `nums[i]` is the **weakest link** in a chain; the value of the chain is limited by it. Maximize the total strength under that constraint.

---

### 🚨 Interview Red Flags

* Not recognizing histogram-style range computation
* Misuse of prefix sum ranges

---

### 🎯 Talking Points

* “I treat each element as the fixed minimum and expand to max range using monotonic stack.”
* “Prefix sum allows me to compute subarray sums in O(1).”

---

## 🧾 Final Cheat Sheet: Stack Patterns Recap

| Problem                  | Pattern Type               | Strategy                   | Time |
| ------------------------ | -------------------------- | -------------------------- | ---- |
| Remove K Digits          | Monotonic Increasing Stack | Greedy digit elimination   | O(n) |
| 132 Pattern              | Monotonic Decreasing Stack | Reverse check w/ min track | O(n) |
| Max Subarray Min-Product | NSEL + NSER + Prefix Sum   | Histogram area analogy     | O(n) |

---
