# 🧠 Leetcode 347 – Top K Frequent Elements

> **Difficulty**: Medium

> **Topic**: Hashmap, Heap, Bucket Sort

> **Link**: [Leetcode 347 – Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

> **Companies**: Amazon, Google, Facebook, Microsoft, Uber, Apple, Adobe

---

## 📌 Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.
You **must solve it in better than O(n log n)** time complexity.

---

## ✅ Key Observations

* You need to find frequency of elements → use a hashmap (manual or via `Counter`).
* You **cannot sort the entire array** → must use a heap or bucket sort.
* This is a **Top-K problem**, similar to real-world stream ranking problems.

---

## ✅ Interview Expectations

During interviews, be prepared to:

* Justify **time and space complexity**.
* Implement **frequency map without using libraries**.
* Choose and explain different methods (heap vs. bucket).
* Discuss **trade-offs** and **edge cases**:

  * What if `k == len(nums)`?
  * What if elements have the same frequency?

---

## 🛠️ Approach 1: HashMap + Min-Heap (Manual Frequency Map)

### 🧮 Time Complexity: `O(n log k)`

### 🧠 Space Complexity: `O(n)`

### ✅ When to Use:

* When `k` is small or moderate.
* Want a good balance between readability and performance.

### 💡 Code

```python
from typing import List
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Build frequency map manually
        frequency_map = {}
        for num in nums:
            frequency_map[num] = frequency_map.get(num, 0) + 1

        # Step 2: Min-heap to track top k elements
        min_heap = []

        for num, freq in frequency_map.items():
            heapq.heappush(min_heap, (freq, num))
            if len(min_heap) > k:
                heapq.heappop(min_heap)

        # Step 3: Extract numbers from heap
        return [num for freq, num in min_heap]
```

---

## 🛠️ Approach 2: HashMap + `heapq.nlargest`

### 🧮 Time Complexity: `O(n log k)`

### ✅ Cleaner but less flexible than manual heap

### ✅ When to Use:

* When allowed to use Python libraries (`heapq.nlargest`).
* Want the **clearest, concise** solution in real-world code.

### 💡 Code

```python
from typing import List
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Manual frequency map
        frequency_map = {}
        for num in nums:
            frequency_map[num] = frequency_map.get(num, 0) + 1

        # Step 2: Use heapq.nlargest to get top k frequent elements
        top_k = heapq.nlargest(k, frequency_map.items(), key=lambda x: x[1])

        # Step 3: Return only the elements
        return [num for num, freq in top_k]
```

---

## 🛠️ Approach 3: Bucket Sort (Optimal `O(n)`)

### 🧮 Time Complexity: `O(n)`

### 🧠 Space Complexity: `O(n)`

### ✅ When to Use:

* When aiming for optimal time.
* To show depth of understanding and algorithmic thinking.

### 💡 Code

```python
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Build frequency map manually
        freq_map = {}
        for num in nums:
            freq_map[num] = freq_map.get(num, 0) + 1

        # Step 2: Create buckets where index = frequency
        max_freq = len(nums)
        buckets = [[] for _ in range(max_freq + 1)]

        for num, freq in freq_map.items():
            buckets[freq].append(num)

        # Step 3: Collect top k frequent elements from buckets
        result = []
        for freq in range(max_freq, 0, -1):
            for num in buckets[freq]:
                result.append(num)
                if len(result) == k:
                    return result
```

---

## 📊 Comparison Table

| Approach                   | Time Complexity | Space Complexity | When to Use                                    |
| -------------------------- | --------------- | ---------------- | ---------------------------------------------- |
| HashMap + Manual Min-Heap  | `O(n log k)`    | `O(n)`           | Most general-purpose, good for interviews      |
| HashMap + `heapq.nlargest` | `O(n log k)`    | `O(n)`           | Clean and readable (if libraries are allowed)  |
| HashMap + Bucket Sort      | `O(n)`          | `O(n)`           | Optimal, impressive, a bit harder to implement |

---

## 🔍 Visual Explanation (Bucket Sort)

For `nums = [1,1,1,2,2,3]` and `k = 2`:

Frequency Map:

```python
{1: 3, 2: 2, 3: 1}
```

Buckets (index = frequency):

```text
Index:       0    1     2     3
Buckets:    []  [3]   [2]   [1]
```

Start from index `3` down to `0`, pick top `k` = 2 → `[1, 2]`.

---

## ✅ Tips for the Interview

* Start with brute-force, then **optimize to meet time constraints**.
* Show understanding of **hashmap**, **heap mechanics**, and **bucket sort**.
* Clearly state **trade-offs** between time, space, and code complexity.
* Talk through edge cases: duplicates, ties in frequency, large `k`.

---

## 🧠 Summary

* Frequency counting is the foundation for all approaches.
* Heap-based methods are fast, safe, and interview-friendly.
* Bucket sort is best for `O(n)` but more advanced.
* Show both **code fluency** and **algorithmic reasoning**.

---

Here are **diagrams** and **visual aids** to help you understand and explain the **Top K Frequent Elements** problem more intuitively — useful during interviews or for personal learning.

---

## 📊 Visual Intuition – Frequency Map

### 🎯 Input:

```python
nums = [1, 1, 1, 2, 2, 3]
k = 2
```

### 🔁 Step 1: Frequency Count

We create a frequency map:

```
Element → Frequency
  1     →    3
  2     →    2
  3     →    1
```

📌 This tells us how often each element appears.

---

## 🧺 Bucket Sort Visualization

We use an array of **buckets**, where each index `i` represents the frequency `i`.

```
Buckets:
Index (Frequency) → Elements
        0         → []
        1         → [3]
        2         → [2]
        3         → [1]
```

We now **traverse the buckets from right to left** (from high to low frequency) and collect the first `k` elements.

🧮 Final result after collecting top 2 frequent:

```
Result: [1, 2]
```

---

## 🔄 Min Heap Visualization

We use a **min-heap** of size `k`:

### Step-by-step heap operations:

1. Insert `(3, 1)` → heap = `[(3, 1)]`
2. Insert `(2, 2)` → heap = `[(2, 2), (3, 1)]`
3. Insert `(1, 3)` → heap exceeds size → remove min → heap = `[(3, 1), (2, 2)]`

✅ Final Heap:

```
[(2, 2), (3, 1)]
```

Extract numbers:

```
Result: [2, 1]
```

📌 Order can vary since both are among the top `k` frequent.

---

## 📐 Diagram Summary Table

| Concept           | Visualization                                          |
| ----------------- | ------------------------------------------------------ |
| **Frequency Map** | `{'1': 3, '2': 2, '3': 1}`                             |
| **Bucket Sort**   | `buckets[3] = [1], buckets[2] = [2], buckets[1] = [3]` |
| **Min Heap**      | `[(2, 2), (3, 1)] → pop → result`                      |

---

## 🧠 Interviewer-Friendly Whiteboard Sketch

If sketching during an interview, draw something like this:

```plaintext
nums = [1, 1, 1, 2, 2, 3], k = 2

Step 1: Frequency Map
1 → 3
2 → 2
3 → 1

Step 2a: Min Heap (size = k)
Insert (3,1) → heap: [(3,1)]
Insert (2,2) → heap: [(2,2), (3,1)]
Insert (1,3) → too big → pop → heap: [(3,1), (2,2)]

Step 2b: Bucket Sort
Buckets = [[], [3], [2], [1]]
Traverse from index 3 → collect [1], then index 2 → collect [2]
Result: [1, 2]
```

---
