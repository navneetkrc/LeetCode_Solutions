
# 🎯 347. Top K Frequent Elements – Complete Interview Guide

**Difficulty:** Medium
**Tags:** Hash Map, Heap, Bucket Sort, Frequency Count
**Asked by:** Google, Amazon, Microsoft, Facebook

---

## 🧩 Problem Statement

Given an integer array `nums` and an integer `k`, return the **k most frequent elements**.
You may return the answer in **any order**.

---

## 🧪 Examples

### Example 1:

```
Input:  nums = [1, 1, 1, 2, 2, 3], k = 2  
Output: [1, 2]
```

### Example 2:

```
Input:  nums = [1], k = 1  
Output: [1]
```

---

## ✅ Constraints

* `1 <= nums.length <= 10^5`
* `-10^4 <= nums[i] <= 10^4`
* `k` is in the range `[1, number of unique elements]`
* The answer is **guaranteed to be unique**

---

## 🚀 Follow-Up Challenge

Design an algorithm **better than** `O(n log n)` time complexity.

---

## 💡 Key Observations (To Share in Interviews)

1. **Hash Map** is ideal to count frequencies of elements → O(n) time.
2. We want the **k elements with highest frequencies** → Use a **heap**.
3. Python’s `heapq.nlargest` gives us the top `k` items based on a custom key efficiently → O(n log k) time.
4. Avoid full sorting (`O(n log n)`), as it doesn’t meet the follow-up requirement.

---

## 🧠 What You Should Share in an Interview

✔️ Use a hash map to build a frequency table.

✔️ Use a heap (min or max) to efficiently track top-k frequent elements.

✔️ Mention alternatives like bucket sort if asked about time-optimized approaches.

✔️ Always extract only the numbers from the (number, frequency) pairs.

✔️ Be clear on **time complexity** and trade-offs.

---

## 🧾 Final Code (With Clear Comments & Interview-Ready Naming)

```python
from typing import List
import heapq

class Solution:
    def topKFrequent(self, numbers: List[int], k: int) -> List[int]:
        # Step 1: Count frequency of each number using a hash map
        frequency_map = {}
        for number in numbers:
            frequency_map[number] = frequency_map.get(number, 0) + 1
        
        # Step 2: Use a heap to find the k elements with the highest frequency
        # Each item in the heap is a (number, frequency) tuple
        top_k_pairs = heapq.nlargest(k, frequency_map.items(), key=lambda item: item[1])

        # Step 3: Extract only the numbers from the (number, frequency) tuples
        most_frequent_elements = [number for number, freq in top_k_pairs]
        return most_frequent_elements
```

---

## 📈 Time and Space Complexity

| Step                  | Complexity                                |
| --------------------- | ----------------------------------------- |
| Frequency Map         | O(n)                                      |
| Heap Top-K Extraction | O(n log k)                                |
| Final List Creation   | O(k)                                      |
| **Total**             | **O(n log k)** (better than O(n log n)) ✅ |
| Space                 | O(n) for hashmap and heap                 |

---

## 📦 Alternatives (If Asked)

| Approach        | Time         | Space | Notes                               |
| --------------- | ------------ | ----- | ----------------------------------- |
| Full Sort       | O(n log n)   | O(n)  | Too slow for large inputs           |
| **Heap (used)** | ✅ O(n log k) | O(n)  | Ideal for top-k problems            |
| Bucket Sort     | O(n)         | O(n)  | Use if k ≈ n and values are bounded |

---

## 🧭 Interview Tips

🔹 Talk through the need for a **hash map** first.

🔹 Mention trade-offs between **heap vs bucket sort**.

🔹 Use **descriptive variable names** for clarity.

🔹 Draw diagrams or frequency tables for illustration.

🔹 Discuss time complexity at each step.

🔹 If asked about real-world analogy: “Like finding the top k trending hashtags on Twitter.”

---

## 🖼️ Visual Intuition

```
Input: [1,1,1,2,2,3], k = 2

Frequency Map:
1 → 3
2 → 2
3 → 1

Heap Top K:
[(1, 3), (2, 2)] → Extract top 2

Final Output:
[1, 2]
```

---

## ✅ Output Guarantees

Since the answer is guaranteed to be unique, no need to handle tie-breaking logic.

---

## 🧮 Wrap-Up

This problem tests:

* Hash map construction
* Heap-based selection
* Time-space trade-off understanding

You can solve this with clean, efficient logic — and it’s **commonly asked at top-tier companies**.

Absolutely! Here's a markdown version of the **Top K Frequent Elements** problem using the **Bucket Sort** approach — which is **more optimal** when `k` is large and the range of values is manageable.

---

# ✏️ Bucket Sort Approach – Top K Frequent Elements

**Difficulty:** Medium
**Technique:** Hash Map + Bucket Sort
**Time Complexity:** O(n)
**Used When:** Need better than O(n log k) and values are not too sparse

---

## 💡 Why Use Bucket Sort?

* The frequency of an element cannot be more than `n` (array size).
* So we can create a list (`buckets`) of size `n + 1`, where each index stores a list of numbers that occurred that many times.
* Iterate the buckets **in reverse** (from highest frequency down) to collect the top k elements.

---

## 🧠 Key Interview Insight

> "We reduce time complexity to linear by using frequency as the index in a bucket list."

Use this when:

* You want to avoid a heap altogether.
* The number of unique elements is high and sorting is inefficient.

---

## 🧾 Bucket Sort – Interview-Ready Code (Well Commented)

```python
from typing import List
from collections import defaultdict

class Solution:
    def topKFrequent(self, numbers: List[int], k: int) -> List[int]:
        # Step 1: Count frequency of each number
        frequency_map = defaultdict(int)
        for number in numbers:
            frequency_map[number] += 1

        # Step 2: Create buckets where index = frequency
        n = len(numbers)
        buckets = [[] for _ in range(n + 1)]
        for number, frequency in frequency_map.items():
            buckets[frequency].append(number)

        # Step 3: Traverse buckets in reverse to collect top k frequent elements
        result = []
        for freq in range(n, 0, -1):  # Start from highest frequency
            for number in buckets[freq]:
                result.append(number)
                if len(result) == k:
                    return result
```

---

## 🧪 Example Walkthrough

Input:

```
nums = [1, 1, 1, 2, 2, 3], k = 2
```

### ➤ Step 1: Frequency Map

```
{1: 3, 2: 2, 3: 1}
```

### ➤ Step 2: Buckets (Index = Frequency)

```
Index | Elements
---------------
0     | []
1     | [3]
2     | [2]
3     | [1]
```

### ➤ Step 3: Reverse Scan for Top K

* Start at index 6, go down to 0
* Pick from non-empty buckets until `k` elements collected
* Result → `[1, 2]`

---

## 📈 Time and Space Complexity

| Step            | Complexity |
| --------------- | ---------- |
| Frequency Map   | O(n)       |
| Bucket Creation | O(n)       |
| Bucket Scan     | O(n)       |
| **Total**       | ✅ **O(n)** |
| Space           | O(n)       |

---

## ✅ Interview Tips

* This is an optimal linear time solution.
* Use when you are asked for a **follow-up** to improve the heap solution.
* Clear justification: “We exploit the fact that frequency can't exceed array length.”

---

## 📦 Heap vs Bucket Sort

| Approach   | Time       | Space | Use When                      |
| ---------- | ---------- | ----- | ----------------------------- |
| Heap       | O(n log k) | O(n)  | k is small                    |
| **Bucket** | ✅ O(n)     | O(n)  | When k is large or n is large |

---

## 🖼️ Diagram

```plaintext
Input: [1, 1, 1, 2, 2, 3], k = 2

Frequency Map:
1 → 3
2 → 2
3 → 1

Buckets:
[
  [],        # 0
  [3],       # 1
  [2],       # 2
  [1],       # 3
  [], [], [] # Up to index = n
]

Reverse bucket scan:
→ [1] (from bucket 3)
→ [1, 2] (from bucket 2)
→ Done
```


# 🧱 Approach 3: Quickselect (Partition-based Top K)

## ✅ Intuition

* Similar to QuickSort’s partitioning logic.
* Build a frequency map, then extract the **unique elements**.
* Partition the list so that the **top k frequent elements** are at the end of the list.
* Quickselect gives **average O(n)** time (worst-case O(n²)).

---

## 🧠 Key Idea

Quickselect helps us avoid full sorting (O(n log n)) by **partially sorting** just enough to get the k most frequent elements.

We:

1. Count frequency.
2. Apply partition logic using frequencies as keys.
3. Return last `k` elements (most frequent ones).

---

## ✅ Interview-Ready Code

```python
from typing import List
from collections import Counter
import random

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Count frequency
        frequency_map = Counter(nums)
        unique_elements = list(frequency_map.keys())  # Unique numbers only

        def partition(left: int, right: int, pivot_index: int) -> int:
            pivot_frequency = frequency_map[unique_elements[pivot_index]]
            # Move pivot to end
            unique_elements[pivot_index], unique_elements[right] = unique_elements[right], unique_elements[pivot_index]
            store_index = left

            for i in range(left, right):
                if frequency_map[unique_elements[i]] < pivot_frequency:
                    unique_elements[store_index], unique_elements[i] = unique_elements[i], unique_elements[store_index]
                    store_index += 1

            # Move pivot to its final place
            unique_elements[right], unique_elements[store_index] = unique_elements[store_index], unique_elements[right]
            return store_index

        def quickselect(left: int, right: int, k_smallest: int):
            if left == right:
                return

            pivot_index = random.randint(left, right)
            pivot_index = partition(left, right, pivot_index)

            if k_smallest == pivot_index:
                return
            elif k_smallest < pivot_index:
                quickselect(left, pivot_index - 1, k_smallest)
            else:
                quickselect(pivot_index + 1, right, k_smallest)

        n = len(unique_elements)
        # kth most frequent is (n - k)th least frequent
        quickselect(0, n - 1, n - k)

        return unique_elements[n - k:]  # Top-k frequent elements
```

---

## 🧪 Example

```
Input: [1,1,1,2,2,3], k = 2
```

* Frequency map: {1: 3, 2: 2, 3: 1}
* Unique elements: \[1, 2, 3]
* Quickselect rearranges so that top 2 frequent elements are at the end: \[3, 2, 1]
* Output: \[2, 1]

---

## ⏱ Time & Space Complexity (Quickselect)

| Step                 | Complexity   |
| -------------------- | ------------ |
| Frequency Map        | O(n)         |
| Quickselect Avg      | ✅ O(n)       |
| Worst-Case Partition | O(n²) (rare) |
| Space                | O(n)         |

---

## 📦 Approach Comparison Table (All 3)

| Approach          | Time Complexity       | Space | Best Use Case                    |
| ----------------- | --------------------- | ----- | -------------------------------- |
| ✅ **Heap**        | O(n log k)            | O(n)  | Good general purpose, k is small |
| ✅ **Bucket Sort** | O(n)                  | O(n)  | Best for large k                 |
| ✅ **Quickselect** | Avg O(n), Worst O(n²) | O(n)  | High-signal, advanced interviews |

---

## 🧭 Final Interview Summary

✅ Mastering **all three** approaches prepares you for:

* Brute force optimization
* Top-K design patterns
* Custom sorting & partitioning logic
* Handling follow-ups with multiple trade-offs

---

## 🧠 Which to Use When?

| Interviewer Says...                                  | Use This Approach                    |
| ---------------------------------------------------- | ------------------------------------ |
| "Can you improve on sorting?"                        | ✅ Heap or Bucket Sort                |
| "Can we do better than O(n log k)?"                  | ✅ Bucket Sort                        |
| "What if you had to implement your own top-k logic?" | ✅ Quickselect                        |
| "What if frequencies were very dense?"               | ✅ Bucket Sort                        |
| "What if memory usage is critical?"                  | ✅ Heap (can be more space-efficient) |

---


## 🖼️ Quickselect Visual Walkthrough

### 🎯 Goal: Find Top **k = 2** frequent elements in

```
nums = [1, 1, 1, 2, 2, 3]
```

---

### 🧩 Step 1: Build Frequency Map

```text
Frequency Map:
1 → 3
2 → 2
3 → 1
```

We convert this into a list of **unique elements**:

```python
unique = [1, 2, 3]
```

---

### 🔀 Step 2: Partition Logic (Quickselect)

We want the **k = 2** most frequent → we need the **(n - k) = 1** least frequent element at position 1.

So we call:

```python
quickselect(unique, left=0, right=2, k_smallest=1)
```

---

### ⚙️ Partition (Random Pivot Example)

Let’s say:

```text
pivot_index = 0 → pivot = unique[0] = 1 (frequency 3)
```

We move the pivot to the end:

```text
Before: [1, 2, 3]
Swap pivot with right → [3, 2, 1]
```

Now we partition based on frequency:

```text
Comparing to pivot frequency = 3
- 3 (freq 1) < 3 → move to left
- 2 (freq 2) < 3 → move to left
```

Swap pivot back to its final spot:

```text
Partitioned: [3, 2, 1] → Pivot ends at index 2
```

---

### 🔁 Step 3: Recurse on Left Part

Since `pivot_index = 2 > k_smallest = 1`, we recurse left:

```python
quickselect(unique, left=0, right=1, k_smallest=1)
```

---

### ⚙️ Partition Again

New pivot = 0 → element = 3 (freq 1)

Swap with right → \[2, 3]
Now:

```text
2 (freq 2) > 1 → skip
```

Final swap:

```text
[3, 2] → pivot ends at index 0
```

---

### 🔁 Recurse Again

Now `pivot_index = 0 < k_smallest = 1`, so recurse right:

```python
quickselect(unique, left=1, right=1, k_smallest=1)
```

✅ Left == Right → Done!

---

### 🎯 Final State of Unique Elements

```text
unique = [3, 2, 1]
```

Take last `k = 2` elements:

```python
return unique[-2:] → [2, 1]
```

✅ These are the **Top 2 Frequent Elements**

---

## 📈 Visual Recap Table

| Step         | Array State | Pivot | Frequency Map   | Partition Result | Next Step      |
| ------------ | ----------- | ----- | --------------- | ---------------- | -------------- |
| Initial Call | \[1, 2, 3]  | 1 → 3 | {1:3, 2:2, 3:1} | \[3, 2, 1]       | Recurse Left   |
| 2nd Call     | \[3, 2]     | 3 → 1 |                 | \[3, 2]          | Recurse Right  |
| Final Call   | \[2]        | -     |                 | Done             | Return \[2, 1] |

---

## 🧭 Key Talking Points for Interviews

✅ “We’re not sorting everything, just positioning the (n-k)th element.”

✅ “Partition logic ensures that higher frequency elements bubble toward the end.”

✅ “Quickselect gives **average O(n)** time – better than sorting and sometimes even better than heap if k ≈ n.”

✅ “Great for large inputs when we want a fine balance between time and space.”

---



