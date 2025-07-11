## 🔢 Problem Statement (Recap)

> Given an integer array `nums`, return the **length of the longest strictly increasing subsequence**.

---

## ✅ Overview of Approaches

| Approach                          | Time Complexity | Space | Notes                                                       |
| --------------------------------- | --------------- | ----- | ----------------------------------------------------------- |
| 1. Recursive (Brute Force)        | O(2ⁿ)           | O(n)  | Exponential time, explores all subsequences                 |
| 2. Top-Down DP (Memoization)      | O(n²)           | O(n²) | Avoids recomputation of overlapping subproblems             |
| 3. Bottom-Up DP (Tabulation)      | O(n²)           | O(n)  | Classic DP approach                                         |
| 4. Patience Sorting (Greedy + BS) | O(n log n)      | O(n)  | Uses binary search to track minimal tails                   |
| 5. Segment Tree / Fenwick Tree    | O(n log n)      | O(n)  | Useful for range queries, less common                       |
| 6. Monotonic Stack (Simulated)    | O(n log n)      | O(n)  | Same as Patience Sorting, interpretable as increasing stack |

---

## 🔍 Detailed Explanation

---

### 🟠 1. **Recursive Brute Force (Backtracking)**

```python
def LIS_recursive(nums, prev, index):
    if index == len(nums):
        return 0
    taken = 0
    if nums[index] > prev:
        taken = 1 + LIS_recursive(nums, nums[index], index + 1)
    not_taken = LIS_recursive(nums, prev, index + 1)
    return max(taken, not_taken)
```

* **Time:** O(2ⁿ)
* **Use when:** Teaching conceptually; impractical for large inputs.

---

### 🟡 2. **Top-Down DP (Memoization)**

```python
from functools import lru_cache

def lengthOfLIS(nums):
    @lru_cache(None)
    def dp(i, prev_index):
        if i == len(nums):
            return 0
        take = 0
        if prev_index == -1 or nums[i] > nums[prev_index]:
            take = 1 + dp(i + 1, i)
        not_take = dp(i + 1, prev_index)
        return max(take, not_take)
    
    return dp(0, -1)
```

* **Time:** O(n²)
* **Space:** O(n²) because of 2D memo table

---

### 🟢 3. **Bottom-Up DP (Tabulation)**

```python
def lengthOfLIS(nums):
    n = len(nums)
    dp = [1] * n  # dp[i] = LIS ending at index i

    for i in range(n):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)
```

* **Time:** O(n²)
* **Space:** O(n)
* **Use when:** You want to understand substructure, or reconstruct the LIS.

---

### 🔵 4. **Greedy + Binary Search (Patience Sorting Approach)**

*(Also interpretable as monotonic increasing stack)*

```python
from bisect import bisect_left

def lengthOfLIS(nums):
    tails = []
    for num in nums:
        idx = bisect_left(tails, num)
        if idx == len(tails):
            tails.append(num)
        else:
            tails[idx] = num
    return len(tails)
```

* **Time:** O(n log n)
* **Space:** O(n)
* **Most efficient** for just LIS length.

---

### 🟣 5. **Segment Tree or Fenwick Tree**

Use **coordinate compression** and a segment tree to do:

* `query max` on range `1…num-1`
* `update` at current number index

```text
Too complex unless range is large and multiple queries are needed.
```

* **Time:** O(n log n)
* **Use when:** Input constraints are high or range queries are needed

---

### 🟤 6. **Monotonic Stack (Simulated)**

This is just a **different interpretation** of the **Patience Sorting / Binary Search** method:

```python
from bisect import bisect_left

def lengthOfLIS(nums):
    incr_stack = []
    for num in nums:
        idx = bisect_left(incr_stack, num)
        if idx == len(incr_stack):
            incr_stack.append(num)
        else:
            incr_stack[idx] = num
    return len(incr_stack)
```

* **Time:** O(n log n)
* **Space:** O(n)
* **Interview Insight:** Emphasize it maintains a **monotonically increasing sequence of minimal tails**.

---

## 🔁 Want to Reconstruct the Actual Sequence?

Modify the DP or greedy solution to track **predecessor indices**. I can share that if needed.

---

## 📊 When to Use What?

| Use Case                             | Recommended Approach                           |
| ------------------------------------ | ---------------------------------------------- |
| Teaching / brute force understanding | Recursive backtracking                         |
| Medium input (n < 1000)              | Bottom-Up DP                                   |
| Large input (n > 10⁴)                | Greedy + Binary Search                         |
| Return actual LIS                    | Modified DP or Patience Sort with backtracking |
| Competitive Programming              | Greedy or Segment Tree                         |

---
Here is a concise and **interview-ready cheat sheet** 🧾 of all major approaches to solve the **Longest Increasing Subsequence (LIS)** problem, with clean and well-commented **code templates**.

---

# 🧠 Longest Increasing Subsequence (LIS) – Code Cheat Sheet

---

## 🔸 1. **Recursive Brute Force**

```python
def lis_brute(nums, prev=float('-inf'), idx=0):
    if idx == len(nums):
        return 0

    taken = 0
    if nums[idx] > prev:
        taken = 1 + lis_brute(nums, nums[idx], idx + 1)

    not_taken = lis_brute(nums, prev, idx + 1)
    return max(taken, not_taken)
```

* 🔴 Time: O(2^n)
* Use only for small inputs or teaching recursion.

---

## 🔸 2. **Top-Down DP (Memoization)**

```python
from functools import lru_cache

def lis_memo(nums):
    n = len(nums)

    @lru_cache(None)
    def dp(i, prev_idx):
        if i == n:
            return 0

        take = 0
        if prev_idx == -1 or nums[i] > nums[prev_idx]:
            take = 1 + dp(i + 1, i)

        skip = dp(i + 1, prev_idx)
        return max(take, skip)

    return dp(0, -1)
```

* 🟡 Time: O(n²), Space: O(n²)

---

## 🔸 3. **Bottom-Up DP (Tabulation)**

```python
def lis_dp(nums):
    n = len(nums)
    dp = [1] * n  # dp[i] = length of LIS ending at i

    for i in range(n):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)
```

* 🟢 Time: O(n²), Space: O(n)
* Easy to extend to reconstruct actual sequence

---

## 🔸 4. **Greedy + Binary Search (Patience Sorting)**

```python
from bisect import bisect_left

def lis_binary_search(nums):
    tails = []  # smallest tail of increasing subsequence of each length

    for num in nums:
        idx = bisect_left(tails, num)
        if idx == len(tails):
            tails.append(num)  # extend the LIS
        else:
            tails[idx] = num   # replace with a smaller tail

    return len(tails)
```

* 🔵 Time: O(n log n), Space: O(n)
* Best for large inputs (n > 10⁴)

---

## 🔸 5. **Monotonic Stack Interpretation (Same as Above)**

```python
from bisect import bisect_left

def lis_monotonic_stack(nums):
    incr_stack = []  # behaves like a monotonic increasing stack

    for num in nums:
        pos = bisect_left(incr_stack, num)
        if pos == len(incr_stack):
            incr_stack.append(num)
        else:
            incr_stack[pos] = num

    return len(incr_stack)
```

* 🟣 Just a different view of the binary search method

---

## 🔸 6. **Reconstruct the Actual LIS**

```python
def lis_with_reconstruction(nums):
    from bisect import bisect_left

    n = len(nums)
    dp = []
    parent = [-1] * n
    idx_at_length = []

    for i, num in enumerate(nums):
        pos = bisect_left(dp, num)
        if pos == len(dp):
            dp.append(num)
            idx_at_length.append(i)
        else:
            dp[pos] = num
            idx_at_length[pos] = i

        if pos > 0:
            parent[i] = idx_at_length[pos - 1]

    # Reconstruct LIS
    lis = []
    k = idx_at_length[-1]
    while k != -1:
        lis.append(nums[k])
        k = parent[k]

    return lis[::-1]
```

* 💡 Use when interviewer asks for actual sequence, not just length

---

## 🔸 7. **Segment Tree / Fenwick Tree** (Advanced)

Useful when input values are in large range and we want fast queries + updates.

```python
# Placeholder – actual implementation is complex and usually overkill
# Coordinate compression + Fenwick Tree or Segment Tree required
```

* 🔴 Not interview-friendly unless explicitly requested

---

## 🧩 Summary Table

| Approach                 | Time       | Space | Sequence Reconstruction? |
| ------------------------ | ---------- | ----- | ------------------------ |
| Brute Force              | O(2^n)     | O(n)  | ❌                        |
| Top-Down DP              | O(n²)      | O(n²) | ❌ (but modifiable)       |
| Bottom-Up DP             | O(n²)      | O(n)  | ✅ Easy to backtrack      |
| Binary Search + Greedy   | O(n log n) | O(n)  | ❌ (unless extended)      |
| Monotonic Stack-Like     | O(n log n) | O(n)  | ❌ (same as above)        |
| Reconstruct LIS (Greedy) | O(n log n) | O(n)  | ✅                        |

---

---

<img width="1536" height="1024" alt="17522091446206758916656592142854" src="https://github.com/user-attachments/assets/6f5c7a82-c5d7-498d-bee4-43897f193864" />

