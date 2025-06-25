# 🥇 215. Kth Largest Element in an Array

**Difficulty:** Medium  
**Topic Tags:** Heap, Quickselect, Sorting  
**Companies:** Amazon, Google, Facebook, Microsoft, Apple

---

## 🧩 Problem Statement

> Given an integer array `nums` and an integer `k`, return the **kth largest element** in the array.  
> Note: It is the kth largest element in the sorted order, **not** the kth distinct element.

---

### 🧪 Examples

#### Example 1:
```

Input:  nums = \[3, 2, 1, 5, 6, 4], k = 2
Output: 5

```
The sorted array is `[1, 2, 3, 4, 5, 6]`. The 2nd largest element is `5`.

#### Example 2:
```

Input:  nums = \[3, 2, 3, 1, 2, 4, 5, 5, 6], k = 4
Output: 4

````
The sorted array is `[1, 2, 2, 3, 3, 4, 5, 5, 6]`. The 4th largest element is `4`.

---

## ✅ Constraints

- `1 <= k <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- **Can you solve it without full sorting?**

---

## 🤔 Intuition

Instead of sorting the entire array (`O(n log n)`), we just want to find the **kth largest** element.  
This can be done efficiently using:

1. **Min-Heap (Priority Queue)** of size `k`:  
   - Keep only the largest `k` elements seen so far.
   - Time: `O(n log k)`, Space: `O(k)`
2. **Quickselect Algorithm**:  
   - Similar to quicksort partitioning.
   - Average Time: `O(n)`, Worst-case: `O(n^2)`.

---

## 🚀 Approach: Min Heap (Efficient & Reliable)

We maintain a **min heap** of size `k`.  
At every step:
- If the heap exceeds size `k`, we remove the smallest.
- The top of the heap will eventually be the `k`th **largest** element.

---

## 🧠 Interviewer Expectations

| Expectation                | What to Say / Do                                                  |
|---------------------------|--------------------------------------------------------------------|
| Understand the problem    | Clarify kth *largest*, not kth smallest or distinct                |
| Optimize for performance  | Discuss why full sorting is inefficient (`O(n log n)`)             |
| Choose efficient approach | Suggest min-heap or quickselect for better average time            |
| Edge cases                | Mention cases like all duplicates, negatives, single element, etc. |
| Write clean code          | Use self-explanatory variable names and concise logic              |

---

## 🧼 Clean & Well-Commented Code (Min-Heap)

```python
from typing import List
import heapq  # Provides heap functionality

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Min-heap to keep track of top k largest elements
        min_heap = []

        for num in nums:
            heapq.heappush(min_heap, num)  # Push current number to heap

            # If heap size exceeds k, remove the smallest element
            if len(min_heap) > k:
                heapq.heappop(min_heap)

        # The root of the heap is the kth largest element
        return min_heap[0]
```

---
## 🧼 Clean & Well-Commented Code (Min-Heap)

```python

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        seen = {}

        for num in nums:
            seen[num] = seen.get(num, 0) + 1

        # Step 2: Use heapq.nlargest to get k elements with highest frequency
        most_common = heapq.nlargest(k, seen.items(), key=lambda pair: pair[1])

        # Step 3: Extract only the numbers (not their frequencies)
        result = [num for num, count in most_common]
        return result

        

```

---

## 📊 Time & Space Complexity

| Metric           | Value        |
| ---------------- | ------------ |
| Time Complexity  | `O(n log k)` |
| Space Complexity | `O(k)`       |

---

## 🧭 Follow-up Questions in Interview

* Can you implement this using QuickSelect? (Follow-up)
* What happens when `k = 1` or `k = len(nums)`?
* Can you do it in-place with constant space?

---

## 🖼️ Visual Example

### Example: nums = \[7, 10, 4, 3, 20, 15], k = 3

We want the **3rd largest** element.

```
Insert into min heap:
[7] → [7, 10] → [4, 10, 7] → [3, 4, 7, 10] → pop 3 → [4, 10, 7]
→ [4, 10, 7, 20] → pop 4 → [7, 10, 20]
→ [7, 10, 20, 15] → pop 7 → [10, 15, 20]

Result = 10
```

---

## 🏁 Conclusion

This problem tests your understanding of:

* Efficient element selection
* Heap data structure
* Time-space trade-offs

Always explain your **thought process, optimizations**, and **edge-case handling** to the interviewer.

---



Let me know if you'd like to see a Quickselect version or visual illustrations for that approach too.
```
