# 📊 Leetcode 1365: How Many Numbers Are Smaller Than the Current Number

---

## 🔍 Problem Description

Given an array `nums`, for each `nums[i]`, find out how many numbers in the array are **smaller than it**.

You need to **return a new array** such that `result[i]` contains the **count of elements `nums[j]`** 

**where `j ≠ i` and `nums[j] < nums[i]`**.

---

### 🧪 Examples

**Example 1**  
Input: `nums = [8,1,2,2,3]`  
Output: `[4,0,1,1,3]`  
Explanation:
- `8` has 4 smaller numbers → `[1,2,2,3]`
- `1` has 0 smaller numbers
- `2` has 1 smaller number → `[1]`
- `2` has 1 smaller number → `[1]`
- `3` has 3 smaller numbers → `[1,2,2]`

---

**Example 2**  
Input: `nums = [6,5,4,8]`  
Output: `[2,1,0,3]`

**Example 3**  
Input: `nums = [7,7,7,7]`  
Output: `[0,0,0,0]`

---

### 📚 Constraints
- `2 <= nums.length <= 500`
- `0 <= nums[i] <= 100`

---

## 🎯 Interviewer Expectations

### ✅ What are they testing?
- Can you identify a brute-force vs optimal solution?
- Are you able to leverage input constraints for optimization?
- Do you write **clean code** with meaningful variable names?
- Can you communicate your approach clearly and handle edge cases?

---

## 🧠 Step-by-Step Intuition

### 🥇 Brute Force (Naive): O(N²)
- For each element, count how many others are smaller.
- **Time Limit Exceeded** in large test cases.
  
```python
def smallerNumbersThanCurrent(nums):
    result = []
    for i in range(len(nums)):
        count = 0
        for j in range(len(nums)):
            if nums[j] < nums[i]:
                count += 1
        result.append(count)
    return result
````

---

### 🥈 Optimized Approach: Frequency Array + Prefix Sum

#### 🚀 Key Insight:

* Since `nums[i]` ranges only from `0` to `100`, we can:

  1. Count frequency of each number.
  2. Build a prefix sum array so that for any number `x`, `prefix[x-1]` tells how many numbers are **strictly less** than `x`.

#### 💡 Example:

`nums = [8, 1, 2, 2, 3]`
Frequencies:

```
index_array[1] = 1  
index_array[2] = 2  
index_array[3] = 1  
index_array[8] = 1  
```

Prefix Sum:

```
prefix[2] = index_array[0] + index_array[1] = 0 + 1 = 1  
```

---

## ✅ Final Optimized Code (With Clean Variables & Comments)

```python
from typing import List

class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        # Step 1: Count frequency of each number in range 0 to 100
        frequency = [0] * 101  # Index i represents number i

        for num in nums:
            frequency[num] += 1

        # Step 2: Build prefix sum to find count of numbers smaller than i
        for i in range(1, 101):
            frequency[i] += frequency[i - 1]

        # Step 3: Construct the result using the prefix sum array
        result = []
        for num in nums:
            if num == 0:
                result.append(0)  # No number less than 0
            else:
                result.append(frequency[num - 1])  # Count of numbers < num

        return result
```

---

## 🔄 Alternative Approaches

### ✅ Sorting-Based with Hash Map

1. Sort a copy of the array.
2. For each unique element, store its first occurrence index in a map.
3. That index represents how many elements are smaller than it.

```python
def smallerNumbersThanCurrent(nums: List[int]) -> List[int]:
    sorted_nums = sorted(nums)
    num_to_count = {}
    
    for idx, val in enumerate(sorted_nums):
        if val not in num_to_count:
            num_to_count[val] = idx  # First occurrence = count of smaller elements
    
    return [num_to_count[num] for num in nums]
```

* ⏱️ Time: O(N log N)
* 🧠 Space: O(N)
* ✅ Also acceptable and readable in interviews

---

## ⚠️ Common Pitfalls

* Forgetting `j ≠ i` in brute force.
* Not using prefix sum correctly (off-by-one errors).
* Using an approach that doesn’t leverage the known input constraint: `0 <= nums[i] <= 100`.

---

## 💬 What You Should Say in the Interview

> 🗣️ "Since the input numbers are bounded between 0 and 100, I can use a frequency array instead of nested loops. By building a prefix sum over the frequency array, I can answer each query in O(1) time. This brings down the total complexity to O(N), which is much better than O(N²). I chose clear variable names and separated each step for better readability and maintenance."

> 🧠 "An alternate approach involves sorting and indexing, which is easier to implement but has O(N log N) complexity."

---

## 🏁 Final Thoughts

* Always **leverage constraints** when optimizing.
* Keep variable names descriptive to improve readability.
* Mention **space-time tradeoffs** during your explanation.
* Share **why your solution scales better** than naive ones.

---



Absolutely! Here's an enhanced and visually rich version of the **final optimized approach** for Leetcode **1365. How Many Numbers Are Smaller Than the Current Number**, with:

* ✅ **Deeper Intuition**
* 🧠 **Step-by-step explanation**
* 🔍 **Worked-out example with visualization**
* 📌 **What makes it optimal**

---

---

# ✅ Final Optimized Approach: Frequency Array + Prefix Sum

---

## 🧠 Core Intuition

We're asked to count how many numbers are **smaller** than each number in the input array `nums`.

Instead of comparing each number with all others (which would be O(n²)), we can **use counting and prefix sums** — a very common trick when input values lie within a known small range.

---

## 🧩 Key Observations

1. **Range Constraint:**
   - All numbers `nums[i]` lie between `0` and `100`.
   - This means we can use a **fixed-size array of size 101** to store frequency information.

2. **Frequency Array (Counting Sort Idea):**
   - Create an array `frequency[0..100]`.
   - Each index `i` holds the **number of times** `i` appears in `nums`.

3. **Prefix Sum Array:**
   - Transform `frequency` to a **prefix sum** array so that:
     - `prefix[i] = number of elements ≤ i`
     - `prefix[i - 1] = number of elements < i`

4. **Final Answer:**
   - For each element `x` in `nums`, the number of smaller elements is `prefix[x - 1]`.

---

## 🔎 Step-by-Step Example

Let's walk through `nums = [6, 1, 2, 2, 9, 3]`.

### 📊 Step 1: Count Frequencies
We count how many times each number appears in `nums`.

```

frequency\[0 to 10] (only showing 0–10 for clarity):
Index:       0  1  2  3  4  5  6  7  8  9 10
Count:       0  1  2  1  0  0  1  0  0  1  0

````

- 1 appears once
- 2 appears twice
- 3 appears once
- 6 appears once
- 9 appears once

---

### ➕ Step 2: Convert to Prefix Sum

We now update `frequency[i]` to hold the number of elements **less than or equal to** `i`.

```text
prefix_sum[i] = frequency[0] + frequency[1] + ... + frequency[i]
````

```
Prefix Sum:
Index:       0  1  2  3  4  5  6  7  8  9 10
Prefix:      0  1  3  4  4  4  5  5  5  6  6
```

Interpretation:

* prefix\[2] = 3 → 3 numbers ≤ 2
* prefix\[6] = 5 → 5 numbers ≤ 6
* prefix\[9] = 6 → all 6 numbers in array ≤ 9

---

### ✅ Step 3: Generate Result

We use `prefix[num - 1]` to count how many numbers are **strictly less** than `num`.

| num | smaller count = prefix\[num - 1] |
| --- | -------------------------------- |
| 6   | prefix\[5] = 4                   |
| 1   | prefix\[0] = 0                   |
| 2   | prefix\[1] = 1                   |
| 2   | prefix\[1] = 1                   |
| 9   | prefix\[8] = 5                   |
| 3   | prefix\[2] = 3                   |

🔚 **Final Result: `[4, 0, 1, 1, 5, 3]`**

---

## ✅ Final Code with Intuition Embedded

```python
from typing import List

class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        # Step 1: Count occurrences of each number
        frequency = [0] * 101  # frequency[i] = how many times i appears in nums

        for num in nums:
            frequency[num] += 1

        # Step 2: Build prefix sum to know how many numbers are <= i
        for i in range(1, 101):
            frequency[i] += frequency[i - 1]

        # Step 3: Build the result using the prefix sum
        result = []
        for num in nums:
            # If num is 0, no number is smaller than it
            if num == 0:
                result.append(0)
            else:
                # frequency[num - 1] tells how many numbers are < num
                result.append(frequency[num - 1])
        return result
```

---

## 🔍 Time & Space Complexity

| Metric   | Value                             |
| -------- | --------------------------------- |
| 🕒 Time  | O(N + K) → N = len(nums), K = 101 |
| 💾 Space | O(K) → constant size = 101        |

⚡ Extremely efficient for up to 500 elements due to small value constraint.

---

## 💬 What to Say in the Interview

> "Given that input numbers are bounded between 0 and 100, I chose to optimize using a frequency array and prefix sum approach. This avoids nested loops and provides O(N) time with constant space. This is a classic example of leveraging constraints to reduce time complexity. I also ensured meaningful variable names and readable logic to demonstrate clean coding practices."

---

## 🧠 Bonus Interview Tip

> 💡 Always mention when you are trading **space for speed** — interviewers love that awareness!

---
