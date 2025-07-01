# 🧠 Stack Interview Preparation Guide: Advanced Problems

---

## 🔷 Pattern: **Monotonic Stack** – Increasing/Decreasing

### Q7. **Daily Temperatures (Temperature Rise)**

* 🔍 **Interview Intent**:

  * Tests understanding of **monotonic decreasing stack**
  * Ability to process **next greater elements with indices**
  * Evaluates space-time optimization via stack

* 📝 **Step-by-step Walkthrough**:

  For `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`

  ```
  Stack stores indices; result initialized to 0s.
  i = 0 → stack = [0]
  i = 1 → 74 > 73 → result[0] = 1; stack = []
         → stack = [1]
  i = 2 → 75 > 74 → result[1] = 1; stack = []
         → stack = [2]
  ...
  Final result: [1,1,4,2,1,1,0,0]
  ```

* ⚠️ **Common Pitfalls**:

  * Storing values instead of **indices** (needed for day difference)
  * Not handling last few elements correctly (remain 0)

* 🔄 **Follow-up Variations**:

  * Nearest greater to left
  * Rainwater Trapping (same stack discipline)

* 💡 **Optimization Hint**:

  * O(n) time via stack; brute force is O(n²)
  * Use stack for **waiting indices**, not values

* ✅ **Template**:

  ```python
  stack = []
  result = [0] * len(temps)
  for i, t in enumerate(temps):
      while stack and temps[i] > temps[stack[-1]]:
          idx = stack.pop()
          result[idx] = i - idx
      stack.append(i)
  ```

* 🧠 **Real-world Analogy**: Think of people waiting for a warmer day — stack stores those still waiting.

---

### Q8. **Stock Span Problem**

* 🔍 **Interview Intent**:

  * Tests monotonic **decreasing stack from left**
  * Recognize **NGEL pattern**

* 📝 **Step-by-step Walkthrough**:

  ```
  prices = [100, 80, 60, 70, 60, 75, 85]
  Stack holds pairs (price, index). Span[i] = i - prev greater index

  i=0: 100 → span=1 → stack=[(100, 0)]
  i=1: 80 < 100 → span=1 → stack=[(100,0),(80,1)]
  i=2: 60 < 80 → span=1 → stack=[...(60,2)]
  i=3: 70 > 60 → pop → span=2 (i-1)
  i=4: 60 → span=1
  i=5: 75 > 60 → pop → 75 > 70 → pop → span=4 (i-1)
  i=6: 85 → pop till 100 → span=6 (i-0)
  ```

* ⚠️ **Common Pitfalls**:

  * Forgetting to subtract **index** of previous higher price
  * Using prices instead of indices

* 🔄 **Follow-up Variants**:

  * Nearest greater to right
  * Price drop problem

* 💡 **Optimization Hint**:

  * Maintain stack with decreasing price order for O(n)

* ✅ **Template**:

  ```python
  span = [0] * n
  stack = []
  for i in range(n):
      while stack and price[i] >= price[stack[-1]]:
          stack.pop()
      span[i] = i + 1 if not stack else i - stack[-1]
      stack.append(i)
  ```

* 🧠 **Real-world Analogy**: Stock buyers comparing today’s price with streak of previous lesser/equal prices.

---

### Q9. **Maximum Area Histogram (MAH)**

* 🔍 **Interview Intent**:

  * Combines NSER + NSEL for span calculation
  * Strong test of problem **composition and pattern reuse**

* 📝 **Walkthrough**:

  * Compute NSER and NSEL:

    ```
    NSER: nearest smaller to right → helps with right limit
    NSEL: nearest smaller to left → helps with left limit
    Width = right - left - 1
    Area = width * height[i]
    ```

* ⚠️ **Common Pitfalls**:

  * Off-by-one in width calculation
  * Wrong default index when stack is empty (NSEL → -1, NSER → n)

* 🔄 **Follow-up Variants**:

  * Largest rectangle under skyline
  * Remove building for max area

* 💡 **Optimization Hint**:

  * Can combine both NSER and NSEL in one pass for speed

* ✅ **Template**:

  ```python
  def NSER(heights):
      stack, right = [], [len(heights)] * len(heights)
      for i in reversed(range(len(heights))):
          while stack and heights[stack[-1]] >= heights[i]:
              stack.pop()
          if stack:
              right[i] = stack[-1]
          stack.append(i)
      return right
  ```

* 🧠 **Real-world Analogy**: Buildings with shadows; height limits visibility → area bounded by next smaller.

---

### Q10. **Max Area Rectangle in Binary Matrix**

* 🔍 **Interview Intent**:

  * Extends MAH to 2D → evaluates abstraction + composition skills
  * Good for dynamic matrix updates

* 📝 **Walkthrough**:

  * For each row:

    * If matrix\[i]\[j] == 1 → heights\[j] += 1
    * Else → heights\[j] = 0
    * Apply MAH on `heights`

* ⚠️ **Common Pitfalls**:

  * Resetting height array incorrectly
  * Forgetting to apply MAH at every row

* 🔄 **Follow-up Variants**:

  * Maximum square sub-matrix
  * Dynamic matrix queries (online updates)

* 💡 **Optimization Hint**:

  * O(rows × cols) with MAH per row

* ✅ **Template**:

  ```python
  for i in range(rows):
      for j in range(cols):
          heights[j] = heights[j] + 1 if matrix[i][j] == 1 else 0
      max_area = max(max_area, MAH(heights))
  ```

* 🧠 **Real-world Analogy**: Think of 1s as stacked boxes — every row adds “layers”.

---

### Q11. **Valid Subarrays**

* 🔍 **Interview Intent**:

  * Monotonic stack (increasing)
  * Conceptual understanding of **subarrays constrained by leftmost element**

* 📝 **Walkthrough**:

  * Count subarrays ending at each index i, where `nums[i]` is the smallest so far

  ```
  nums = [1, 4, 2, 5, 3]
  Use increasing stack:
  For each i, pop until nums[i] < nums[stack[-1]]
  Count += i - last_smaller_index
  ```

* ⚠️ **Common Pitfalls**:

  * Not recognizing that leftmost ≤ rest implies increasing stack
  * Miscounting valid spans

* 🔄 **Follow-up Variants**:

  * Count subarrays with all elements ≥ leftmost
  * Count maximums/minimums using Monotonic Stack + Sliding Window

* 💡 **Optimization Hint**:

  * O(n) stack approach; brute force O(n²)

* ✅ **Template**:

  ```python
  stack = []
  count = 0
  for i in range(len(nums)):
      while stack and nums[i] < nums[stack[-1]]:
          stack.pop()
      count += i - stack[-1] if stack else i + 1
      stack.append(i)
  ```

* 🧠 **Real-world Analogy**: Group of employees where team lead (leftmost) must not be outshone by others.

---

## 🧾 Cheat Sheet: Stack Patterns

| Pattern | Use Case         | Stack Behavior       | Common Problem |
| ------- | ---------------- | -------------------- | -------------- |
| NGEL    | Previous greater | Monotonic decreasing | Stock Span     |
| NGER    | Next greater     | Monotonic decreasing | Daily Temp     |
| NSEL    | Previous smaller | Monotonic increasing | MAH            |
| NSER    | Next smaller     | Monotonic increasing | MAH            |

---

## 🚨 Red Flags in Interviews

| Mistake               | Interviewer Reads It As     |
| --------------------- | --------------------------- |
| Brute force O(n²)     | Poor algorithmic judgment   |
| Doesn’t mention stack | Missed pattern recognition  |
| Can’t explain dry run | Surface-level understanding |

---

## 🗣️ Talking Points to Showcase Strength

* “This is a classic monotonic stack problem, which optimizes linear traversal using conditional popping.”
* “We only push to the stack when we know this element has unresolved conditions.”

---

## ⏳ Time Management Tip

* Don’t jump to code. First, clarify the pattern (NSEL/NSER/NGER/NGEL)
* Handle edge cases in code *before* testing

---

## 🧠 Pattern Mastery Tracker

| Problem                      | Pattern(s) Used      | Level |
| ---------------------------- | -------------------- | ----- |
| Daily Temperatures           | NGER                 | Easy  |
| Stock Span                   | NGEL                 | Easy  |
| Max Area Histogram           | NSEL + NSER          | Med   |
| Max Area Rectangle in Matrix | NSEL + NSER (MAH)    | Hard  |
| Valid Subarrays              | Monotonic Increasing | Med   |

---
