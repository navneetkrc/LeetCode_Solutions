# Sliding Window Patterns - Complete Study Guide

*Based on Greg Hogg's YouTube Playlist*

## Overview

The sliding window technique is a powerful algorithmic pattern used to solve problems involving arrays, strings, and sequences. This technique helps optimize brute force solutions from O(n²) or O(n³) time complexity to O(n) in many cases.

**Core Concept**: Instead of recalculating values for each subarray/substring, we maintain a "window" and slide it across the data structure, adding new elements and removing old ones as needed.

## Video Playlist

📺 **Main Playlist**: [Sliding Window Patterns by Greg Hogg](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W)

---

## Problems & Solutions

### 1. Max Consecutive Ones III
- **🎥 Video**: [Tutorial #1](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=1)
- **💻 LeetCode**: [Problem 1004 - Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/description/)
- **Difficulty**: Medium
- **Key Concept**: Variable-size sliding window with constraint (maximum k flips)
- **Pattern**: Expand window until constraint is violated, then shrink from left

### 2. Longest Substring Without Repeating Characters
- **🎥 Video**: [Tutorial #2](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=2)
- **💻 LeetCode**: [Problem 3 - Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- **Difficulty**: Medium
- **Key Concept**: Variable-size sliding window with character frequency tracking
- **Pattern**: Use hash map to track character positions, adjust window when duplicates found

### 3. Longest Repeating Character Replacement
- **🎥 Video**: [Tutorial #3](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=3)
- **💻 LeetCode**: [Problem 424 - Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)
- **Difficulty**: Medium
- **Key Concept**: Variable-size sliding window with character replacement limit
- **Pattern**: Track most frequent character in current window, replace others up to k times

### 4. Minimum Size Subarray Sum
- **🎥 Video**: [Tutorial #4](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=4)
- **💻 LeetCode**: [Problem 209 - Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)
- **Difficulty**: Medium
- **Key Concept**: Variable-size sliding window with sum constraint
- **Pattern**: Expand window until sum >= target, then shrink to find minimum length

### 5. Maximum Average Subarray I
- **🎥 Video**: [Tutorial #5](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=5)
- **💻 LeetCode**: [Problem 643 - Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
- **Difficulty**: Easy
- **Key Concept**: Fixed-size sliding window
- **Pattern**: Classic sliding window - slide window of size k and track maximum sum

### 6. Permutation in String
- **🎥 Video**: [Tutorial #6](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=6)
- **💻 LeetCode**: [Problem 567 - Permutation in String](https://leetcode.com/problems/permutation-in-string/)
- **Difficulty**: Medium
- **Key Concept**: Fixed-size sliding window with character frequency matching
- **Pattern**: Use frequency map to check if current window is a permutation of target

---

## Key Sliding Window Patterns

### 1. Fixed-Size Sliding Window
**When to use**: Problems asking for operations on subarrays/substrings of a specific size k.

**Template**:
```
Initialize window of size k
Calculate initial result
For each new element:
    Add new element to window
    Remove element that's k positions back
    Update result
```

### 2. Variable-Size Sliding Window
**When to use**: Problems asking for optimal subarray/substring with certain constraints.

**Template**:
```
left = 0
For right in range(n):
    Add arr[right] to window
    While window violates constraint:
        Remove arr[left] from window
        left += 1
    Update result with current window
```

### 3. Two-Pointer Sliding Window
**When to use**: Problems involving string/array matching or finding pairs.

**Template**:
```
left = 0, right = 0
While right < n:
    Expand window by moving right
    While condition is met:
        Update result
        Shrink window by moving left
    right += 1
```

---

## Common Problem Types

### String Problems
- Finding longest/shortest substring with conditions
- Anagram/permutation matching
- Character frequency problems

### Array Problems
- Subarray sum problems
- Finding maximum/minimum in sliding windows
- Consecutive elements problems

### Optimization Scenarios
- Convert O(n²) brute force to O(n)
- Problems with "contiguous" or "consecutive" keywords
- Finding optimal subarray/substring

---

## Tips for Success

1. **Identify the Pattern**: Look for keywords like "subarray", "substring", "consecutive", or "contiguous"

2. **Choose Window Type**: 
   - Fixed size: When problem specifies exact window size
   - Variable size: When looking for optimal window with constraints

3. **Track State Efficiently**: Use hash maps for frequency counting, variables for sums/counts

4. **Handle Edge Cases**: Empty arrays, single elements, impossible constraints

5. **Practice Variations**: Different constraint types, multiple conditions, optimization targets

---

## Study Approach

1. **Master the Basics**: Start with fixed-size sliding window problems
2. **Understand Templates**: Learn the common patterns and when to apply them
3. **Practice Progressively**: Move from easy to hard problems
4. **Analyze Time Complexity**: Understand why sliding window improves performance
5. **Code Different Variations**: Practice both string and array problems

---

## Additional Resources

- **Greg Hogg's Channel**: [YouTube Channel](https://www.youtube.com/channel/UCKJNzy_GuvX3SAg3HlOsIJQ)
- **LeetCode Sliding Window Tag**: Practice more problems with this pattern
- **AlgoMap**: Structured learning path for coding interviews

Remember: The sliding window technique is about optimizing repeated calculations by reusing previous computations. Master this concept and you'll solve dozens of problems efficiently!
