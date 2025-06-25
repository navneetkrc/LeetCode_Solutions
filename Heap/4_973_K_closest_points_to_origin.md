# 📍 973. K Closest Points to Origin – Complete Interview Guide

**Difficulty:** Medium
**Tags:** Heap, Sorting, Geometry, Divide and Conquer
**Asked in:** Google, Amazon, Bloomberg

---

## 🧩 Problem Statement

You're given a list of `points` where each point is represented by `[x, y]`, and an integer `k`. Return the `k` points closest to the **origin** `(0, 0)` using **Euclidean distance**.

### 📐 Distance Formula

$$
\text{Distance from origin} = \sqrt{x^2 + y^2}
$$

> Since square root preserves order, we can **compare squared distances** to save computation.

---

### ✅ Example 1:

**Input:**
`points = [[1,3], [-2,2]]`, `k = 1`
**Output:**
`[[-2,2]]`

**Explanation:**

* (1,3): Distance = √(1² + 3²) = √10
* (-2,2): Distance = √(4 + 4) = √8
  → Return closer one: `[-2, 2]`

---

### ✅ Example 2:

**Input:**
`points = [[3,3], [5,-1], [-2,4]]`, `k = 2`
**Output:**
`[[3,3],[-2,4]]` *(any order)*

---

## 🧠 Interviewer Expectations

> ✅ Be sure to **discuss multiple approaches** before coding.

### 🚀 Key Discussion Points

| Observation         | Discussion                                  |
| ------------------- | ------------------------------------------- |
| Distance comparison | Skip `sqrt()` to improve performance        |
| Efficient selection | Use a **max heap** of size `k`              |
| Return type         | Return a list of `k` closest `[x, y]` pairs |
| Time complexity     | Should aim for **O(n log k)** using a heap  |

---

## ✅ Optimal Heap-Based Solution (Python)

```python
import heapq
from typing import List

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        # Max heap to keep track of k closest points (negate distance for max-heap simulation)
        max_heap = []

        def squared_distance(x: int, y: int) -> int:
            # Compute squared Euclidean distance to origin
            return x * x + y * y

        for x, y in points:
            dist = squared_distance(x, y)

            # Push to heap: store negative distance to mimic max-heap behavior
            heapq.heappush(max_heap, (-dist, x, y))

            # If we have more than k elements, remove the farthest one
            if len(max_heap) > k:
                heapq.heappop(max_heap)

        # Extract coordinates from the heap
        return [[x, y] for (_, x, y) in max_heap]
```

---

## 🔍 Intuition and Visual Walkthrough

### 📊 Visual: Squared Distance Comparison

| Point   | Distance² to (0,0) |
| ------- | ------------------ |
| (1, 3)  | 1² + 3² = **10**   |
| (-2, 2) | (-2)² + 2² = **8** |

---

### 🧺 Why Use a Max-Heap?

Because we always want to **discard the farthest point** if more than `k` points are in the heap.

**Max-heap logic**:

* Push: `(-distance, x, y)`
* Keep heap size at `k`
* Farthest point (largest distance) gets removed

---

## ⏱️ Time and Space Complexity

| Aspect | Complexity                                                          |
| ------ | ------------------------------------------------------------------- |
| Time   | **O(n log k)** – for maintaining a heap of size `k` over `n` points |
| Space  | **O(k)** – for storing `k` closest points                           |

---

## 💬 What to Say in the Interview

✅ Before coding, say:

* “We can solve this via sorting in O(n log n), but we’ll optimize with a heap for O(n log k).”
* “We'll use a max heap so we can easily discard the farthest point once the heap exceeds `k`.”
* “We avoid the square root function to save computation – since distances are only compared.”

✅ After coding, add:

* “This is efficient when `k` is small compared to `n`.”
* “If `k ≈ n`, sorting might be more efficient in practice.”
* “This approach ensures uniqueness in result size and gives any valid ordering.”

---

## 🧪 Bonus: Alternative Approaches

### 1. Sorting (Simple but Slower)

```python
points.sort(key=lambda point: point[0]**2 + point[1]**2)
return points[:k]
```

⏱️ Time: O(n log n)
📦 Space: O(1) or O(n) depending on language sort

---

### 2. Quickselect (Advanced, Avg O(n))

More efficient but trickier to implement in interviews. Only use if you're very comfortable.

---

## ✅ Summary

| Feature          | Heap-based    |
| ---------------- | ------------- |
| Time Complexity  | O(n log k)    |
| Space Complexity | O(k)          |
| Best Use Case    | When `k << n` |
| Interview Ready? | ✅ YES         |

---



# 🎯 K Closest Points to Origin - Complete Interview Guide

## 📋 Problem Description

**LeetCode 973 - K Closest Points to Origin (Medium)**

Given an array of points where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the X-Y plane is the **Euclidean distance**: `√((x₁ - x₂)² + (y₁ - y₂)²)`

Since we're measuring distance from origin `(0, 0)`, the formula simplifies to: `√(x² + y²)`

### 📝 Examples

```
Example 1:
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]

Explanation:
• Distance from (1,3) to origin = √(1² + 3²) = √10 ≈ 3.16
• Distance from (-2,2) to origin = √((-2)² + 2²) = √8 ≈ 2.83
• Since √8 < √10, (-2,2) is closer

Example 2:
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]

Distance Analysis:
• (3,3): √(9 + 9) = √18 ≈ 4.24
• (5,-1): √(25 + 1) = √26 ≈ 5.10  
• (-2,4): √(4 + 16) = √20 ≈ 4.47
• Two closest: (3,3) and (-2,4)
```

### 🔒 Constraints
- `1 <= k <= points.length <= 10⁴`
- `-10⁴ <= xi, yi <= 10⁴`
- Answer order doesn't matter
- Answer is guaranteed to be unique

---

## 💡 Solution Approach

### 🧠 Key Insights
1. **Distance Optimization**: We don't need actual distance, just relative comparison
2. **Squared Distance**: `x² + y²` is sufficient (avoids expensive sqrt operation)
3. **Max-Heap Strategy**: Maintain k smallest distances using max-heap
4. **Space Efficiency**: Only store k elements, not all n elements

### 📊 Algorithm Strategy
```
Core Idea: Use Max-Heap of size K
• Keep k closest points in heap
• When heap full, compare with largest distance in heap
• If new point closer, replace the farthest point
• Result: Heap contains k closest points
```

---

## 🔧 Optimized Solution

```python
from typing import List
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        
        def calculate_squared_distance_from_origin(x_coordinate, y_coordinate):
            """
            Calculate squared distance from origin (0,0) to point (x,y)
            We use squared distance to avoid expensive sqrt operations
            Since we only need relative comparisons, x² + y² is sufficient
            """
            return x_coordinate * x_coordinate + y_coordinate * y_coordinate
        
        # Max heap to maintain k closest points
        # Python heapq is min-heap, so we negate distances to simulate max-heap
        max_heap_of_k_closest_points = []
        
        for current_point in points:
            x_coordinate, y_coordinate = current_point[0], current_point[1]
            
            # Calculate squared distance (no sqrt needed for comparison)
            squared_distance_from_origin = calculate_squared_distance_from_origin(x_coordinate, y_coordinate)
            
            if len(max_heap_of_k_closest_points) < k:
                # Heap not full yet, add current point
                # Use negative distance to simulate max-heap behavior
                heapq.heappush(
                    max_heap_of_k_closest_points, 
                    (-squared_distance_from_origin, x_coordinate, y_coordinate)
                )
            else:
                # Heap is full, compare with farthest point in heap
                # If current point is closer than the farthest, replace it
                farthest_distance_in_heap = -max_heap_of_k_closest_points[0][0]
                
                if squared_distance_from_origin < farthest_distance_in_heap:
                    # Current point is closer, replace the farthest point
                    heapq.heappushpop(
                        max_heap_of_k_closest_points,
                        (-squared_distance_from_origin, x_coordinate, y_coordinate)
                    )
        
        # Extract points from heap (ignore distances)
        k_closest_points_result = []
        for heap_entry in max_heap_of_k_closest_points:
            negative_distance, x_coord, y_coord = heap_entry
            k_closest_points_result.append([x_coord, y_coord])
        
        return k_closest_points_result
```

---

## 🎭 Alternative Approaches

### 🥇 Approach 1: Max-Heap (Current - Optimal for Most Cases)
```python
# Time: O(n log k), Space: O(k)
# Best when k is much smaller than n
def kClosest_maxheap(self, points: List[List[int]], k: int) -> List[List[int]]:
    max_heap = []
    
    for x, y in points:
        squared_dist = x*x + y*y
        
        if len(max_heap) < k:
            heapq.heappush(max_heap, (-squared_dist, x, y))
        else:
            heapq.heappushpop(max_heap, (-squared_dist, x, y))
    
    return [[x, y] for _, x, y in max_heap]
```

### 🥈 Approach 2: Min-Heap (All Points)
```python
# Time: O(n log n), Space: O(n)
# Simple but less efficient for large n, small k
def kClosest_minheap(self, points: List[List[int]], k: int) -> List[List[int]]:
    min_heap = []
    
    for x, y in points:
        squared_dist = x*x + y*y
        heapq.heappush(min_heap, (squared_dist, x, y))
    
    result = []
    for _ in range(k):
        _, x, y = heapq.heappop(min_heap)
        result.append([x, y])
    
    return result
```

### 🥉 Approach 3: Sorting
```python
# Time: O(n log n), Space: O(n)
# Most intuitive but not optimal
def kClosest_sorting(self, points: List[List[int]], k: int) -> List[List[int]]:
    # Sort points by squared distance from origin
    points_with_distance = []
    for x, y in points:
        squared_dist = x*x + y*y
        points_with_distance.append((squared_dist, [x, y]))
    
    points_with_distance.sort(key=lambda item: item[0])
    
    return [point for _, point in points_with_distance[:k]]
```

### 🏆 Approach 4: QuickSelect (Advanced)
```python
# Time: O(n) average, O(n²) worst, Space: O(1)
# Most efficient for large datasets
def kClosest_quickselect(self, points: List[List[int]], k: int) -> List[List[int]]:
    def squared_distance(point):
        return point[0]**2 + point[1]**2
    
    def partition(left, right, pivot_index):
        pivot_dist = squared_distance(points[pivot_index])
        points[pivot_index], points[right] = points[right], points[pivot_index]
        
        store_index = left
        for i in range(left, right):
            if squared_distance(points[i]) < pivot_dist:
                points[store_index], points[i] = points[i], points[store_index]
                store_index += 1
        
        points[right], points[store_index] = points[store_index], points[right]
        return store_index
    
    def quickselect(left, right, k):
        if left == right:
            return
        
        pivot_index = left + (right - left) // 2
        pivot_index = partition(left, right, pivot_index)
        
        if k == pivot_index:
            return
        elif k < pivot_index:
            quickselect(left, pivot_index - 1, k)
        else:
            quickselect(pivot_index + 1, right, k)
    
    quickselect(0, len(points) - 1, k)
    return points[:k]
```

---

## 🗣️ Interview Communication Strategy

### 🎤 What to Say to the Interviewer

#### 1. **Problem Understanding** (45 seconds)
> "I need to find the k closest points to the origin (0,0) using Euclidean distance. Let me think about this step by step:
> 
> For each point (x,y), the distance to origin is √(x² + y²). However, since I only need to compare distances, I can work with squared distances x² + y² to avoid expensive square root calculations.
> 
> The key insight is that I don't need to sort all points - I just need the k smallest distances."

#### 2. **Approach Analysis** (1-2 minutes)
> "I can think of several approaches:
> 
> 1. **Sorting**: Calculate distances, sort, take first k → O(n log n)
> 2. **Min-heap**: Add all points to heap, extract k smallest → O(n log n)  
> 3. **Max-heap of size k**: Maintain only k closest points → O(n log k)
> 4. **QuickSelect**: Partition-based selection → O(n) average
> 
> For this problem, I'll use the max-heap approach because:
> - It's O(n log k) which is better than O(n log n) when k << n
> - It uses O(k) space instead of O(n)
> - It's more intuitive than QuickSelect for interviews"

#### 3. **Key Technical Decisions** (1 minute)
> "Important optimizations I'm making:
> 1. **Squared distance only**: Avoiding sqrt since we only need comparisons
> 2. **Max-heap simulation**: Using negative values with Python's min-heap
> 3. **heappushpop**: Efficient one-operation replacement when heap is full
> 4. **Space optimization**: Only storing k points, not all n points"

#### 4. **Code Walkthrough** (2-3 minutes)
> "Let me code this step by step:
> 1. For each point, I calculate squared distance
> 2. If heap has less than k points, I add the current point
> 3. If heap is full, I compare with the farthest point in heap
> 4. If current point is closer, I replace the farthest point
> 5. Finally, I extract all points from the heap"

#### 5. **Complexity Analysis** (30 seconds)
> "Time complexity: O(n log k) where we process n points and each heap operation is log k.
> Space complexity: O(k) for the heap.
> This is optimal when k is much smaller than n, which is the common case."

---

## 🎯 Key Interview Points to Mention

### ✅ Technical Highlights
- **Distance Optimization**: Using squared distance instead of actual distance
- **Heap Strategy**: Max-heap vs min-heap trade-offs
- **Space-Time Trade-off**: O(n log k) vs O(n log n) complexity
- **Practical Considerations**: When to use each approach based on k vs n

### 🔍 Follow-up Questions You Might Get

#### Q: "What if k is very large, close to n?"
> "If k is close to n, the sorting approach O(n log n) might be better than heap O(n log k). We could also use min-heap to add all points and extract k smallest."

#### Q: "Can you optimize further?"
> "Yes! QuickSelect can achieve O(n) average time by partitioning around the k-th smallest distance. It's like QuickSort but we only recurse on one side."

#### Q: "What about memory constraints?"
> "The max-heap approach is already memory-optimal at O(k). If we need O(1) extra space, we'd need QuickSelect with in-place partitioning."

#### Q: "How would you handle very large datasets?"
> "For massive datasets, I'd consider:
> - External sorting if data doesn't fit in memory
> - Distributed computing for parallel processing
> - Approximate algorithms if exact k closest isn't required"

### 🚨 Common Mistakes to Avoid
- Don't calculate actual square root (expensive and unnecessary)
- Don't sort all points when k is small
- Don't forget to negate distances for max-heap simulation
- Don't ignore the space complexity advantage of heap approach
- Don't mix up min-heap vs max-heap logic

---

## 🧪 Test Cases & Edge Cases

```python
def test_solution():
    solution = Solution()
    
    # Test Case 1: Basic example
    points1 = [[1,3],[-2,2]]
    k1 = 1
    result1 = solution.kClosest(points1, k1)
    print(f"Test 1: {result1}")  # Expected: [[-2,2]]
    
    # Test Case 2: Multiple points
    points2 = [[3,3],[5,-1],[-2,4]]
    k2 = 2
    result2 = solution.kClosest(points2, k2)
    print(f"Test 2: {result2}")  # Expected: [[3,3],[-2,4]] in any order
    
    # Test Case 3: k equals all points
    points3 = [[1,1],[2,2],[3,3]]
    k3 = 3
    result3 = solution.kClosest(points3, k3)
    print(f"Test 3: {result3}")  # Expected: all points
    
    # Test Case 4: Points at origin
    points4 = [[0,0],[1,1],[2,2]]
    k4 = 1
    result4 = solution.kClosest(points4, k4)
    print(f"Test 4: {result4}")  # Expected: [[0,0]]
    
    # Test Case 5: Negative coordinates
    points5 = [[-5,-5],[-1,-1],[1,1]]
    k5 = 2
    result5 = solution.kClosest(points5, k5)
    print(f"Test 5: {result5}")  # Expected: [[-1,-1],[1,1]]

# Run tests
test_solution()
```

---

## 📊 Visual Distance Comparison

```
Example: points = [[3,3],[5,-1],[-2,4]], k = 2

Distance Calculations:
Point (3,3):   √(3² + 3²) = √(9 + 9) = √18 ≈ 4.24
Point (5,-1):  √(5² + (-1)²) = √(25 + 1) = √26 ≈ 5.10
Point (-2,4):  √((-2)² + 4²) = √(4 + 16) = √20 ≈ 4.47

Ranking by distance:
1st closest: (3,3) with distance ≈ 4.24
2nd closest: (-2,4) with distance ≈ 4.47  
3rd closest: (5,-1) with distance ≈ 5.10

Result for k=2: [[3,3], [-2,4]]
```

---

## 🏆 Success Metrics

**You've aced the interview if you:**
- ✅ Immediately recognized this as a "top-k" problem
- ✅ Explained why squared distance is sufficient
- ✅ Discussed multiple approaches and their trade-offs
- ✅ Chose the optimal approach based on constraints
- ✅ Implemented clean code with descriptive variable names
- ✅ Correctly analyzed time/space complexity
- ✅ Handled follow-up questions about optimization
- ✅ Demonstrated understanding of heap data structures
- ✅ Considered edge cases and practical constraints

**Red Flags to Avoid:**
- ❌ Calculating unnecessary square roots
- ❌ Sorting when heap would be more efficient
- ❌ Confusing min-heap vs max-heap logic
- ❌ Not considering space complexity
- ❌ Ignoring the k vs n relationship in complexity analysis

**Remember**: Show your problem-solving methodology, not just coding skills!
