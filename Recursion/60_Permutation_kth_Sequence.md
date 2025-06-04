# 60. Permutation Sequence

## Problem Statement

**Difficulty:** Hard

The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations..

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:
```
"123"
"132"
"213"
"231"
"312"
"321"
```

Given `n` and `k`, return the `kth` permutation sequence.

### Examples

**Example 1:**
- Input: `n = 3, k = 3`
- Output: `"213"`

**Example 2:**
- Input: `n = 4, k = 9`
- Output: `"2314"`

**Example 3:**
- Input: `n = 3, k = 1`
- Output: `"123"`

### Constraints
- `1 <= n <= 9`
- `1 <= k <= n!`

---

## Key Insights

### 🧠 Core Mathematical Insight
The problem can be solved using the **Factorial Number System**:

1. **Grouping Concept**: For `n` numbers, permutations can be grouped by their first digit
   - Each group contains `(n-1)!` permutations
   - For `n=4`: Group 1️⃣ has `3! = 6` perms, Group 2️⃣ has `6` perms, etc.

2. **Direct Calculation**: We can determine which number goes in each position without generating all permutations
   - Position of first digit: `⌈k / (n-1)!⌉ - 1`
   - Remaining problem: Find `(k % (n-1)!)th` permutation of remaining numbers

3. **Edge Case Optimization**: When remainder is 0, we need the lexicographically largest arrangement (descending order)

### 🎯 Why This Approach Works
- **Efficiency**: `O(n²)` vs `O(n! × n)` for brute force
- **Direct Computation**: No need to generate intermediate permutations
- **Mathematical Foundation**: Uses properties of factorial number system

---

## Solution Approaches

## Approach 1: Recursive Solution (Optimized)

### Algorithm Overview
1. Pre-compute factorials for efficiency
2. Use recursive function to build permutation digit by digit
3. Handle the special case where remainder = 0 by adding remaining elements in descending order

```python
import math

class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        """
        Find the kth permutation sequence of numbers 1 to n using recursion.
        
        Key Insight: Use factorial number system to directly compute positions
        without generating all permutations.
        
        Time Complexity: O(n²) - due to list pop operations
        Space Complexity: O(n) - recursion stack + nums_list
        """
        
        # Pre-compute factorials from 0! to 9! to avoid repeated calculations
        # factorial[i] = i! 
        factorial = [1] * 10
        for i in range(1, 10):
            factorial[i] = factorial[i-1] * i
        
        # Initialize list of available numbers [1, 2, 3, ..., n]
        nums_list = list(range(1, n + 1))
        
        # Result list to build the permutation
        res = []
        
        def add_to_string(nums_list, n, k):
            """
            Recursive function to build the kth permutation
            
            Args:
                nums_list: List of available numbers to choose from
                n: Current length of nums_list  
                k: Position we're looking for (1-indexed)
            """
            
            # Base case: only one number left
            if n == 1:
                res.append(str(nums_list[0]))
                return
            
            # Calculate which number should be at current position
            # Each group has (n-1)! permutations
            group_size = factorial[n - 1]
            
            # Find which group the kth permutation belongs to
            # Using math.ceil and subtracting 1 to handle 1-indexing correctly
            index = math.ceil(k / group_size) - 1
            
            # Add the chosen number to result
            res.append(str(nums_list[index]))
            
            # Remove the chosen number from available numbers
            nums_list.pop(index)
            
            # Calculate remainder for the subproblem
            remainder = k % group_size
            
            # If remainder is 0, we want the last permutation of remaining numbers
            # This means we need all remaining elements in descending order
            if remainder == 0:
                # Add all remaining numbers in reverse sorted order (descending)
                # Pop from the end and add to result to get descending order
                while nums_list:
                    res.append(str(nums_list.pop()))
                return  # Exit recursion as we've added all remaining elements
            
            # Recursive call with updated parameters
            add_to_string(nums_list, n - 1, remainder)
        
        # Start the recursive process
        add_to_string(nums_list, n, k)
        
        return ''.join(res)
```

### 🔍 Step-by-Step Example
For `n=4, k=9`:

1. **Initial**: `nums_list = [1,2,3,4]`, need 9th permutation
2. **Step 1**: `group_size = 3! = 6`
   - `index = ⌈9/6⌉ - 1 = 2 - 1 = 1` 
   - Pick `nums_list[1] = 2`
   - `remainder = 9 % 6 = 3`
   - `nums_list = [1,3,4]`

3. **Step 2**: `group_size = 2! = 2`
   - `index = ⌈3/2⌉ - 1 = 2 - 1 = 1`
   - Pick `nums_list[1] = 3` 
   - `remainder = 3 % 2 = 1`
   - `nums_list = [1,4]`

4. **Step 3**: `group_size = 1! = 1`
   - `index = ⌈1/1⌉ - 1 = 1 - 1 = 0`
   - Pick `nums_list[0] = 1`
   - `remainder = 1 % 1 = 0`
   - Since remainder = 0, add remaining in descending order: `4`

**Result**: `"2314"` ✅

---

## Approach 2: Iterative Solution (0-indexed)

### Algorithm Overview
Convert to 0-indexed system for cleaner arithmetic, then iterate through positions.

```python
class SolutionIterative:
    def getPermutation(self, n: int, k: int) -> str:
        """
        Iterative approach with 0-indexed system for cleaner logic.
        
        Time Complexity: O(n²) - due to list pop operations
        Space Complexity: O(n) - for nums_list and result
        """
        
        # Pre-compute factorials
        factorial = [1] * 10
        for i in range(1, 10):
            factorial[i] = factorial[i-1] * i
        
        # Convert to 0-indexed for cleaner arithmetic
        k -= 1
        
        # Initialize available numbers
        nums_list = list(range(1, n + 1))
        result = []
        
        # Process each position from left to right
        for i in range(n, 0, -1):
            # Calculate index of number to pick for current position
            index = k // factorial[i - 1]
            
            # Add chosen number to result
            result.append(str(nums_list[index]))
            
            # Remove chosen number from available numbers
            nums_list.pop(index)
            
            # Update k for next iteration (remaining permutations to skip)
            k %= factorial[i - 1]
        
        return ''.join(result)
```

### 🔍 Step-by-Step Example
For `n=4, k=9` (converted to `k=8` in 0-indexed):

1. **Position 1**: `i=4`, `factorial[3] = 6`
   - `index = 8 // 6 = 1`
   - Pick `nums_list[1] = 2`
   - `k = 8 % 6 = 2`

2. **Position 2**: `i=3`, `factorial[2] = 2`  
   - `index = 2 // 2 = 1`
   - Pick `nums_list[1] = 3`
   - `k = 2 % 2 = 0`

3. **Position 3**: `i=2`, `factorial[1] = 1`
   - `index = 0 // 1 = 0` 
   - Pick `nums_list[0] = 1`
   - `k = 0 % 1 = 0`

4. **Position 4**: `i=1`, `factorial[0] = 1`
   - `index = 0 // 1 = 0`
   - Pick `nums_list[0] = 4`

**Result**: `"2314"` ✅

---

## Comparison of Approaches

| Aspect | Recursive | Iterative |
|--------|-----------|-----------|
| **Readability** | Natural problem decomposition | Straightforward loop |
| **Edge Case Handling** | Explicit optimization for remainder=0 | Handled naturally by 0-indexing |
| **Space Complexity** | O(n) recursion stack | O(1) auxiliary space |
| **Index Arithmetic** | 1-indexed (requires ceiling) | 0-indexed (simpler division) |
| **Performance** | Slight optimization for last permutation | Consistent O(n²) |

---

## Test Cases & Verification

```python
def test_solutions():
    """Test both approaches with various cases"""
    
    recursive_sol = Solution()
    iterative_sol = SolutionIterative()
    
    test_cases = [
        (3, 1, "123"),   # First permutation
        (3, 3, "213"),   # Middle permutation  
        (3, 6, "321"),   # Last permutation
        (4, 9, "2314"),  # Given example
        (4, 1, "1234"),  # First of n=4
        (4, 24, "4321"), # Last of n=4
    ]
    
    print("Testing both solutions:")
    for n, k, expected in test_cases:
        rec_result = recursive_sol.getPermutation(n, k)
        iter_result = iterative_sol.getPermutation(n, k)
        
        rec_status = "✅" if rec_result == expected else "❌"
        iter_status = "✅" if iter_result == expected else "❌"
        
        print(f"n={n}, k={k}:")
        print(f"  Recursive: {rec_result} {rec_status}")
        print(f"  Iterative: {iter_result} {iter_status}")
        print(f"  Expected:  {expected}")
        print()

test_solutions()
```

---

## Key Takeaways

### ✨ Mathematical Elegance
- **Factorial Number System**: Maps perfectly to permutation ordering
- **No Brute Force**: Direct calculation without generating intermediate results
- **Optimal Complexity**: Best possible for this problem given the constraints

### 🚀 Implementation Insights  
- **Pre-compute Factorials**: Avoid repeated calculations
- **Handle Edge Cases**: Remainder = 0 requires special handling
- **Choose Indexing**: 0-indexed arithmetic is cleaner than 1-indexed

### 🎯 When to Use Each Approach
- **Recursive**: When you want explicit handling of the "last permutation" optimization
- **Iterative**: When you prefer straightforward, consistent logic without edge cases

Both solutions demonstrate the power of mathematical insight in algorithm design, transforming a potentially exponential problem into an efficient polynomial-time solution.
