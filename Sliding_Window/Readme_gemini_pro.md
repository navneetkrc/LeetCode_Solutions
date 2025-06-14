# Mastering the Sliding Window Pattern: A Guide Based on Greg Hogg's Playlist

This document provides a structured guide to the **Sliding Window** algorithmic pattern, using the excellent video playlist created by [Greg Hogg](https://www.youtube.com/@gregphogg).

**Full Playlist Link:** [Sliding Window Patterns by Greg Hogg](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W)

## What is the Sliding Window Pattern?

The Sliding Window is a powerful technique used for problems that involve finding an optimal or specific subarray or substring within a larger array or string. It's highly efficient, typically reducing a brute-force O(n²) or O(n³) solution to an optimal O(n) solution.

The core idea is to maintain a "window" (a contiguous range of elements) and "slide" it across the data structure. Instead of re-computing values for every possible subarray, you cleverly add elements to one end of the window and remove them from the other, saving a massive amount of computation.

## The General Template (Dynamic Window)

Most sliding window problems involve a dynamic window that grows and shrinks. Here is a common template:

```python
def sliding_window_template(arr, ...):
    left = 0
    result = ... # Initialize result (e.g., 0, infinity, etc.)
    current_window_state = ... # e.g., a hash map for counts, a running sum

    for right in range(len(arr)):
        # 1. EXPAND the window by adding the right element
        # Update current_window_state with arr[right]

        # 2. SHRINK the window from the left while a condition is not met
        while window_is_invalid(current_window_state):
            # Update current_window_state by removing arr[left]
            left += 1

        # 3. UPDATE the result with the current valid window
        # result = max(result, right - left + 1) or some other logic
    
    return result
```

## Problems from the Playlist

Here are the specific problems covered in the playlist, organized with direct links to the LeetCode problem and the corresponding video explanation.

| Problem Name                                           | Key Idea / Window Condition                                | LeetCode Problem                                                              | Video Explanation                                                                                                 |
| ------------------------------------------------------ | ---------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Max Consecutive Ones III**                           | Find the longest subarray with at most `k` zeros.          | [Link](https://leetcode.com/problems/max-consecutive-ones-iii/)               | [Watch on YouTube](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=1)     |
| **Longest Substring Without Repeating Characters**     | Find the longest substring with all unique characters.     | [Link](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | [Watch on YouTube](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=2)     |
| **Longest Repeating Character Replacement**            | Find the longest substring where `window_length - max_frequency <= k`. | [Link](https://leetcode.com/problems/longest-repeating-character-replacement/) | [Watch on YouTube](https://www.youtube.com/watch?v=00F2dfA4m8Q&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=3)     |
| **Minimum Size Subarray Sum**                          | Find the shortest subarray whose sum is `≥ target`.        | [Link](https://leetcode.com/problems/minimum-size-subarray-sum/)              | [Watch on YouTube](https://www.youtube.com/watch?v=DfljaUwA9rs&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=4)     |
| **Maximum Average Subarray I**                         | Find the max average of a subarray of a fixed size `k`.    | [Link](https://leetcode.com/problems/maximum-average-subarray-i/)             | [Watch on YouTube](https://www.youtube.com/watch?v=56hBFy63BLA&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=5)     |
| **Permutation in String**                              | Check if a fixed-size window is a permutation of another string. | [Link](https://leetcode.com/problems/permutation-in-string/)                  | [Watch on YouTube](https://www.youtube.com/watch?v=K_wWB39zV3A&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=6)     |

## Key Takeaways

*   **Look for Keywords:** Problems involving "subarray," "substring," "contiguous," "longest," "shortest," or "minimum" are often candidates for the sliding window pattern.
*   **Identify the Window Type:** Determine if the window is of a **fixed size** (like in *Maximum Average Subarray I*) or a **dynamic size** that grows and shrinks based on a condition.
*   **Define the "Invalid" State:** The most crucial part of the dynamic window template is clearly defining the condition that makes a window "invalid," as this is what triggers the shrinking of the window from the left.
