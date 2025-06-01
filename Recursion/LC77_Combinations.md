# Visual Guide to Solving "Combinations" Problem (LeetCode 77)

## Problem Statement

Given two integers `n` and `k`, return **all possible combinations** of `k` numbers chosen from the range `[1, n]`. Return the answer in **any order**.

---

## ✳️ Example Inputs

### Example 1:

* **Input:** `n = 4`, `k = 2`
* **Output:** `[[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]`
* **Explanation:** 4 choose 2 = 6 combinations

### Example 2:

* **Input:** `n = 1`, `k = 1`
* **Output:** `[[1]]`

---

## 🔍 Approach 1: Backtracking (Basic)

### Algorithm:

1. Use DFS to explore each decision: pick or skip the current number.
2. Maintain a temporary list `temp` and add to result when its size is `k`.

### Python Code:

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        def backtrack(start, path):
            if len(path) == k:
                res.append(path[:])
                return
            for i in range(start, n+1):
                path.append(i)
                backtrack(i+1, path)
                path.pop()
        backtrack(1, [])
        return res
```

### Visualization (n = 4, k = 2):

```
Start []
├── [1]
│   ├── [1,2] ✅
│   ├── [1,3] ✅
│   └── [1,4] ✅
├── [2]
│   ├── [2,3] ✅
│   └── [2,4] ✅
└── [3]
    └── [3,4] ✅
```

### Pros:

* Simple and readable
* Easy to implement

### Cons:

* May explore unfruitful paths if not pruned

---

## ⚡ Approach 2: Backtracking with Pruning

### Optimization:

Before diving into the loop, prune impossible paths:

```python
if (n - i + 1) < (k - len(path)):
    break
```

### Optimized Code:

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        def backtrack(start, path):
            if len(path) == k:
                res.append(path[:])
                return
            for i in range(start, n+1):
                if (n - i + 1) < (k - len(path)):
                    break
                path.append(i)
                backtrack(i+1, path)
                path.pop()
        backtrack(1, [])
        return res
```

### Pros:

* Avoids unnecessary recursion
* Faster for large `n`

### Cons:

* Slightly more complex

---

## ⚙️ Approach 3: Recursive DFS without Loop (Using indices)

### Code:

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        temp = []

        def dfs(i, count):
            if count == k:
                res.append(temp[:])
                return
            if (n - i) < (k - count):
                return
            # Skip i+1
            dfs(i + 1, count)
            # Pick i+1
            temp.append(i + 1)
            dfs(i + 1, count + 1)
            temp.pop()

        dfs(0, 0)
        return res
```

### Visualization:

* Decision tree: pick or skip each number
* Efficient depth-first generation of combinations

### Pros:

* Elegant and deep control
* Space-efficient (no loop stack)

### Cons:

* Requires understanding of recursion indices

---

## 🧠 Comparison Table

| Approach                    | Time Complexity | Space Complexity | Pruning | Simplicity |
| --------------------------- | --------------- | ---------------- | ------- | ---------- |
| Basic Backtracking          | O(C(n, k) \* k) | O(k)             | ❌       | ✅          |
| Backtracking + Pruning      | O(C(n, k) \* k) | O(k)             | ✅       | ✅          |
| DFS Recursive (i+1 control) | O(C(n, k) \* k) | O(k)             | ✅       | ⚠️         |

---

## ✅ Recommendation

* Use **Approach 2** for interviews and performance.
* Use **Approach 3** if you want more recursion control or are building on advanced logic.

---

Here's a **tree visualization** in Markdown format for generating all combinations of **choosing 3 elements from 5** (`n = 5`, `k = 3`):

---

### 🌳 Combinations Tree (n = 5, k = 3)

Each level makes a decision: **Pick** or **Skip** the current number.

```
[]
├── Pick 1
│   ├── Pick 2
│   │   ├── Pick 3 → [1,2,3] ✅
│   │   ├── Pick 4 → [1,2,4] ✅
│   │   └── Pick 5 → [1,2,5] ✅
│   ├── Pick 3
│   │   ├── Pick 4 → [1,3,4] ✅
│   │   └── Pick 5 → [1,3,5] ✅
│   ├── Pick 4
│   │   └── Pick 5 → [1,4,5] ✅
│   └── Skip 5 (not enough left) ❌
├── Pick 2
│   ├── Pick 3
│   │   ├── Pick 4 → [2,3,4] ✅
│   │   └── Pick 5 → [2,3,5] ✅
│   ├── Pick 4
│   │   └── Pick 5 → [2,4,5] ✅
│   └── Skip 5 ❌
├── Pick 3
│   └── Pick 4
│       └── Pick 5 → [3,4,5] ✅
└── Skip 4, 5 (not enough elements left) ❌
```

---

### ✅ Final Result

```python
[
 [1,2,3], [1,2,4], [1,2,5],
 [1,3,4], [1,3,5], [1,4,5],
 [2,3,4], [2,3,5], [2,4,5],
 [3,4,5]
]
```

Let me know if you'd like a **color-coded HTML version**, **LaTeX tree**, or a **flowchart-style image**!

---
# 🎯 Complete Guide to Combinations Problem - All Approaches

## Problem Statement
Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

---

## Approach 1: Basic Backtracking (Most Intuitive)

### Algorithm
- Start with an empty combination
- At each step, try adding each possible number
- When combination size reaches `k`, add to result
- Backtrack by removing the last added number

### Code
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    result = []
    
    def backtrack(start, current_combination):
        # Base case: if we have k numbers, add to result
        if len(current_combination) == k:
            result.append(current_combination[:])  # Make a copy
            return
        
        # Try all numbers from start to n
        for i in range(start, n + 1):
            current_combination.append(i)           # Choose
            backtrack(i + 1, current_combination)   # Explore
            current_combination.pop()               # Unchoose (backtrack)
    
    backtrack(1, [])
    return result
```

### Visualization for n=4, k=2
```
                    []
                  /  |  |  \
               [1]   [2] [3] [4]
              / | |   |   |
          [1,2][1,3][1,4][2,3][2,4][3,4]
            ✓    ✓    ✓    ✓    ✓    ✓
```

### Step-by-step Execution
```
Call backtrack(1, [])
├─ i=1: [1] → backtrack(2, [1])
│  ├─ i=2: [1,2] → len=2, add to result ✓
│  ├─ i=3: [1,3] → len=2, add to result ✓  
│  └─ i=4: [1,4] → len=2, add to result ✓
├─ i=2: [2] → backtrack(3, [2])
│  ├─ i=3: [2,3] → len=2, add to result ✓
│  └─ i=4: [2,4] → len=2, add to result ✓
├─ i=3: [3] → backtrack(4, [3])
│  └─ i=4: [3,4] → len=2, add to result ✓
└─ i=4: [4] → backtrack(5, [4]) → no valid combinations
```

**Result:** `[[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]`

### Complexity Analysis
- **Time:** O(C(n,k) × k) - Generate C(n,k) combinations, each taking O(k) to copy
- **Space:** O(k) for recursion depth + O(C(n,k) × k) for result storage

---

## Approach 2: Optimized Backtracking with Pruning

### Algorithm Enhancement
Add pruning condition to avoid exploring impossible branches:
- If remaining elements < elements still needed → prune branch

### Code
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    result = []
    
    def backtrack(start, current_combination):
        # Pruning: if remaining elements < needed elements
        remaining_elements = n - start + 1
        needed_elements = k - len(current_combination)
        if remaining_elements < needed_elements:
            return  # Impossible to form valid combination
            
        # Base case
        if len(current_combination) == k:
            result.append(current_combination[:])
            return
        
        for i in range(start, n + 1):
            current_combination.append(i)
            backtrack(i + 1, current_combination)
            current_combination.pop()
    
    backtrack(1, [])
    return result
```

### Pruning Visualization for n=5, k=3
```
Without Pruning:           With Pruning:
      []                        []
   /  |  |  \              /   |   |   \
 [1] [2] [3] [4]         [1]  [2] [3]  [4]❌
 /|\ /|\ /|\  |          /|\  /|\  /|\  
...  ... ... [4,5]❌    ... ... ... PRUNED!
```

**Pruning Logic:**
- At start=4, current=[], needed=3, remaining=2 → 2<3 → PRUNE!
- At start=5, current=[1], needed=2, remaining=1 → 1<2 → PRUNE!

### Complexity Analysis
- **Time:** O(C(n,k) × k) - Same as basic, but with fewer branches explored
- **Space:** O(k) for recursion + O(C(n,k) × k) for result
- **Advantage:** Significant speedup through early termination

---

## Approach 3: Iterative using Bit Manipulation

### Algorithm
- Use bit patterns to represent combinations
- Each bit position represents whether element is included
- Generate all patterns with exactly k bits set

### Code
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    result = []
    
    # Generate all numbers from 0 to 2^n - 1
    for mask in range(1 << n):
        if bin(mask).count('1') == k:  # Check if exactly k bits are set
            combination = []
            for i in range(n):
                if mask & (1 << i):  # Check if i-th bit is set
                    combination.append(i + 1)
            result.append(combination)
    
    return result
```

### Visualization for n=4, k=2
```
Decimal | Binary | Bits Set | Numbers    | Valid?
--------|--------|----------|------------|--------
   3    |  0011  |    2     | [1, 2]     |   ✓
   5    |  0101  |    2     | [1, 3]     |   ✓
   6    |  0110  |    2     | [2, 3]     |   ✓
   9    |  1001  |    2     | [1, 4]     |   ✓
  10    |  1010  |    2     | [2, 4]     |   ✓
  12    |  1100  |    2     | [3, 4]     |   ✓
  15    |  1111  |    4     | [1,2,3,4]  |   ❌
```

### Step-by-step Process
1. **mask = 3 (0011):** bits 0,1 set → [1,2] ✓
2. **mask = 5 (0101):** bits 0,2 set → [1,3] ✓  
3. **mask = 6 (0110):** bits 1,2 set → [2,3] ✓
4. **mask = 9 (1001):** bits 0,3 set → [1,4] ✓
5. **mask = 10 (1010):** bits 1,3 set → [2,4] ✓
6. **mask = 12 (1100):** bits 2,3 set → [3,4] ✓

### Complexity Analysis
- **Time:** O(2^n × n) - Check all 2^n combinations, each taking O(n) to process
- **Space:** O(C(n,k) × k) for result storage
- **Disadvantage:** Inefficient for large n due to 2^n factor

---

## Approach 4: Iterative using Next Lexicographic Combination

### Algorithm
- Start with first combination [1,2,...,k]
- Generate next combination by finding rightmost element that can be incremented
- Continue until no more combinations possible

### Code
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    result = []
    combination = list(range(1, k + 1))  # Start with [1,2,...,k]
    
    while True:
        result.append(combination[:])  # Add current combination
        
        # Find the rightmost element that can be incremented
        i = k - 1
        while i >= 0 and combination[i] == n - k + 1 + i:
            i -= 1
        
        if i < 0:  # No more combinations
            break
            
        # Increment the found element
        combination[i] += 1
        
        # Reset all elements to the right
        for j in range(i + 1, k):
            combination[j] = combination[i] + (j - i)
    
    return result
```

### Visualization for n=4, k=2
```
Step | Combination | i | Action
-----|-------------|---|------------------
  1  |   [1, 2]    | - | Add to result
  2  |   [1, 2]    | 1 | combination[1] can be incremented (2 < 4)
  3  |   [1, 3]    | - | Add to result  
  4  |   [1, 3]    | 1 | combination[1] can be incremented (3 < 4)
  5  |   [1, 4]    | - | Add to result
  6  |   [1, 4]    | 1 | combination[1] cannot be incremented (4 == 4)
  7  |   [1, 4]    | 0 | combination[0] can be incremented (1 < 3)
  8  |   [2, 3]    | - | Reset combination[1] = 2 + 1 = 3
  9  |   [2, 3]    | 1 | combination[1] can be incremented (3 < 4)
 10  |   [2, 4]    | - | Add to result
 11  |   [2, 4]    | 1 | combination[1] cannot be incremented
 12  |   [2, 4]    | 0 | combination[0] can be incremented (2 < 3)
 13  |   [3, 4]    | - | Reset combination[1] = 3 + 1 = 4
 14  |   [3, 4]    | 1 | combination[1] cannot be incremented
 15  |   [3, 4]    | 0 | combination[0] cannot be incremented (3 == 3)
 16  |     -       | - | i < 0, break
```

### Complexity Analysis
- **Time:** O(C(n,k) × k) - Generate each combination in O(k) time
- **Space:** O(k) for working combination + O(C(n,k) × k) for result
- **Advantage:** Memory efficient, generates combinations in lexicographic order

---

## Approach 5: Recursive with Mathematical Formula

### Algorithm
- Use the mathematical property: C(n,k) = C(n-1,k-1) + C(n-1,k)
- Combinations including n + combinations not including n

### Code
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    if k == 0:
        return [[]]
    if k > n:
        return []
    if k == n:
        return [list(range(1, n + 1))]
    
    # C(n,k) = C(n-1,k-1) + C(n-1,k)
    # Combinations including n
    with_n = []
    for combo in self.combine(n - 1, k - 1):
        with_n.append(combo + [n])
    
    # Combinations not including n  
    without_n = self.combine(n - 1, k)
    
    return with_n + without_n
```

### Visualization for n=4, k=2
```
C(4,2)
├─ Include 4: C(3,1) + [4]
│  ├─ Include 3: C(2,0) + [3] = [[]] + [3] = [[3]]
│  ├─ Exclude 3: C(2,1)
│  │  ├─ Include 2: C(1,0) + [2] = [[]] + [2] = [[2]]  
│  │  └─ Exclude 2: C(1,1) = [[1]]
│  └─ Result: [[3], [2], [1]] → Add 4: [[3,4], [2,4], [1,4]]
└─ Exclude 4: C(3,2)
   ├─ Include 3: C(2,1) + [3]
   │  ├─ Include 2: [[]] + [2] = [[2]]
   │  └─ Exclude 2: [[1]]  
   │  └─ Result: [[2], [1]] → Add 3: [[2,3], [1,3]]
   └─ Exclude 3: C(2,2) = [[1,2]]
   └─ Final: [[2,3], [1,3], [1,2]]

Final Result: [[3,4], [2,4], [1,4], [2,3], [1,3], [1,2]]
```

### Complexity Analysis
- **Time:** O(C(n,k) × k) - Each combination generated with O(k) copying
- **Space:** O(n+k) recursion depth + O(C(n,k) × k) for result
- **Advantage:** Elegant mathematical approach, easy to understand

---

## Approach 6: Built-in Library Solution

### Algorithm
Use Python's `itertools.combinations` which implements optimized C code.

### Code
```python
from itertools import combinations

def combine(self, n: int, k: int) -> List[List[int]]:
    return [list(combo) for combo in combinations(range(1, n + 1), k)]
```

### Complexity Analysis
- **Time:** O(C(n,k) × k) - Optimized C implementation
- **Space:** O(C(n,k) × k) for result storage
- **Advantage:** Fastest in practice, well-tested, concise

---

## 📊 Comprehensive Comparison

| Approach | Time Complexity | Space Complexity | Pros | Cons |
|----------|----------------|------------------|------|------|
| **Basic Backtracking** | O(C(n,k) × k) | O(k + C(n,k)×k) | ✅ Intuitive<br>✅ Easy to understand | ❌ No optimization<br>❌ Explores dead branches |
| **Optimized Backtracking** | O(C(n,k) × k) | O(k + C(n,k)×k) | ✅ Pruning optimization<br>✅ Faster in practice<br>✅ Easy to modify | ❌ Still recursive overhead |
| **Bit Manipulation** | O(2^n × n) | O(C(n,k) × k) | ✅ No recursion<br>✅ Interesting approach | ❌ Exponential time<br>❌ Inefficient for large n |
| **Next Lexicographic** | O(C(n,k) × k) | O(k + C(n,k)×k) | ✅ Memory efficient<br>✅ Iterative<br>✅ Lexicographic order | ❌ Complex logic<br>❌ Hard to understand |
| **Mathematical Recursive** | O(C(n,k) × k) | O(n+k + C(n,k)×k) | ✅ Elegant<br>✅ Mathematical insight | ❌ Deep recursion<br>❌ Redundant calculations |
| **Built-in Library** | O(C(n,k) × k) | O(C(n,k) × k) | ✅ Fastest<br>✅ Most concise<br>✅ Well-tested | ❌ Black box<br>❌ Not learning-oriented |

---

## 🎯 Recommendations

### For Learning and Interviews:
1. **Start with Basic Backtracking** - Understand the core concept
2. **Master Optimized Backtracking** - Learn pruning techniques
3. **Understand the pruning condition** - Key optimization insight

### For Production Code:
1. **Use Built-in Library** (`itertools.combinations`) - Fastest and most reliable
2. **Fallback to Optimized Backtracking** - If library not available

### For Specific Constraints:
- **Small n (≤10):** Any approach works
- **Large n (≥15):** Avoid bit manipulation, prefer optimized backtracking
- **Memory constrained:** Next lexicographic approach
- **Need lexicographic order:** Next lexicographic or basic backtracking with sorting

### Key Insights:
1. **Pruning is crucial** - The condition `(n-i) < (k-count)` provides significant speedup
2. **Backtracking is most versatile** - Easy to modify for variations
3. **Mathematical understanding helps** - C(n,k) = C(n-1,k-1) + C(n-1,k) property
4. **Choose based on context** - Interview vs production vs learning goals

Would you like a flowchart or tree diagram visualization of the recursive calls for any of the approaches?

Yes for Approach 2, make tree diagram first
