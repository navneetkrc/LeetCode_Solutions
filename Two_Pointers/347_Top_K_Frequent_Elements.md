# 🔝 LeetCode 347: Top K Frequent Elements (Medium)

## 📌 Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.
You may return the answer in **any order**.

---

## 🧩 Example

**Input:**
`nums = [1,1,1,2,2,3], k = 2`
**Output:**
`[1,2]`

**Explanation:**

* Element `1` occurs 3 times
* Element `2` occurs 2 times
* Element `3` occurs 1 time
  Top 2 frequent = `[1,2]`

---

## 📐 Constraints

* `1 <= len(nums) <= 10^5`
* `-10^4 <= nums[i] <= 10^4`
* `1 <= k <= len(nums)`

---

## 🚀 Approaches

### 🟠 Approach 1: Sorting by Frequency (O(n log n))

```python
from typing import List
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequency_map = Counter(nums)   # Count occurrences of each number
        sorted_items = sorted(frequency_map.items(), key=lambda x: x[1], reverse=True)
        return [num for num, _ in sorted_items[:k]]
```

✅ Simple, clear
❌ Sorting adds `O(n log n)`

---

### 🟡 Approach 2: Heap (Better for Large Data)

```python
from typing import List
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequency_map = {}
        for value in nums:
            frequency_map[value] = frequency_map.get(value, 0) + 1

        # Use max-heap to get k elements with highest frequency
        most_common_pairs = heapq.nlargest(k, frequency_map.items(), key=lambda pair: pair[1])

        result = [num for num, freq in most_common_pairs]
        return result
```

✅ Uses `heapq.nlargest` → efficient for top K
✅ Time: O(n log k), Space: O(n)

---

### 🟢 Approach 3: Bucket Sort (Optimal O(n))

```python
from typing import List
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequency_map = defaultdict(int)
        for value in nums:
            frequency_map[value] += 1

        # Buckets: index = frequency, value = list of numbers
        buckets = [[] for _ in range(len(nums) + 1)]
        for number, freq in frequency_map.items():
            buckets[freq].append(number)

        # Collect results from high frequency to low
        result = []
        for freq in range(len(buckets) - 1, 0, -1):
            for number in buckets[freq]:
                result.append(number)
                if len(result) == k:
                    return result
```

✅ Time: O(n)
✅ Space: O(n)
✅ Optimal approach often expected in interviews

---

## 🎯 Interview Tips

* Always clarify: **Do we need sorted by frequency or any order?**
* Mention **trade-offs**:

  * Sorting → simple but O(n log n)
  * Heap → good for large n when k is small
  * Bucket Sort → best theoretical O(n)
* Use **meaningful variable names**:

  * `frequency_map` instead of `seen`
  * `most_common_pairs` instead of `most_common`
  * `result` instead of ambiguous names like `res`

---

⚡ **Recommended Answer in Interviews:**

* Start with **heap (Approach 2)** for clarity.
* If pushed for optimization → explain **bucket sort (Approach 3)**.

---

