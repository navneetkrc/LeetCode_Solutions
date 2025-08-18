# 🌀 LeetCode 189: Rotate Array (Medium)

## 📌 Problem Statement

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k ≥ 0`.
Rotation means shifting elements cyclically.

---

## 🧩 Example

**Input:**
`nums = [1,2,3,4,5,6,7], k = 3`
**Output:**
`[5,6,7,1,2,3,4]`

**Explanation:**

* Rotate 1 step → `[7,1,2,3,4,5,6]`
* Rotate 2 steps → `[6,7,1,2,3,4,5]`
* Rotate 3 steps → `[5,6,7,1,2,3,4]` ✅

---

## 📐 Constraints

* `1 <= len(nums) <= 10^5`
* `-2^31 <= nums[i] <= 2^31 - 1`
* `0 <= k <= 10^5`

---

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/c705efb7-4627-44ba-b5e9-721818e6b3bc" />

---

## 🚀 Approaches

### 🔴 Approach 1: Brute Force (❌ TLE)

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        k %= len(nums)
        for _ in range(k):
            last_element = nums[-1]
            for idx in range(len(nums)):
                nums[idx], last_element = last_element, nums[idx]
```

* **Time:** O(n × k) → ❌ too slow
* **Space:** O(1)

---

### 🟠 Approach 2: Extra Array (O(n) Space)

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        rotated = [0] * n
        for idx in range(n):
            rotated[(idx + k) % n] = nums[idx]
        nums[:] = rotated
```

* **Time:** O(n)
* **Space:** O(n)
* **Intuition:** Store shifted values in a new array and copy back.

---

### 🟡 Approach 3: Cyclic Replacements (O(1) Space)

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n
        start_index, rotated_count = 0, 0

        while rotated_count < n:
            current_index, current_value = start_index, nums[start_index]
            while True:
                next_index = (current_index + k) % n
                nums[next_index], current_value = current_value, nums[next_index]
                current_index = next_index
                rotated_count += 1
                if start_index == current_index:
                    break
            start_index += 1
```

* **Time:** O(n)
* **Space:** O(1)
* **Intuition:** Place each number at its correct position in cycles.

---

### 🟢 Approach 4: Reversal Method (Best & Cleanest)

```python
class Solution:
    def reverse(self, nums: List[int], left: int, right: int) -> None:
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left, right = left + 1, right - 1

    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n

        # Step 1: Reverse whole array
        self.reverse(nums, 0, n - 1)
        # Step 2: Reverse first k elements
        self.reverse(nums, 0, k - 1)
        # Step 3: Reverse remaining elements
        self.reverse(nums, k, n - 1)
```

* **Time:** O(n)
* **Space:** O(1)
* **Intuition:** Reverse parts to simulate rotation.

---

## 🎯 Interview Tips

* Always **mod k with n** → avoids unnecessary rotations.
* Be ready to discuss **time vs space trade-offs**.
* The **reversal method** is usually the most expected in interviews.
* Use **meaningful variable names** (e.g., `rotated_count`, `current_index`, `last_element`) to make code interview-friendly.

---

⚡ Recommended Answer in Interview: **Approach 4 (Reversal)** – clean, O(n), O(1).

---
