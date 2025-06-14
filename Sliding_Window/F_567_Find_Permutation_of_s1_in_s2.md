# 🔍 LeetCode 567: Permutation in String - Complete Interview Guide

## 📋 Problem Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

**In other words:** Return `true` if one of `s1`'s permutations is a substring of `s2`.

### 🔢 Examples

```
Example 1:
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba")

Example 2:
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

### ⚡ Constraints
- `1 <= s1.length, s2.length <= 10^4`
- `s1` and `s2` consist of lowercase English letters only

---
## 📊 Visualization

### 🔁 Sliding Window Animation for:

`s1 = "ab"` and `s2 = "eidbaooo"`

```
Step-by-step window moves:

"ei" -> "id" -> "db" -> "ba" ✅ match found!
```

✅ Window "ba" is a permutation of "ab"

---
## 🧠 Intuition

This is a classic **Sliding Window + Frequency Count** problem:

- We’re asked to detect if any **permutation of s1** exists as a substring in `s2`.
- Since permutations have the **same frequency of characters**, the goal is to check for any substring in `s2` of length `len(s1)` that has the same character frequency as `s1`.


---

## 🧠 Key Insights & Approach

### 💡 Core Understanding
- **Permutation** means same characters with same frequencies, but different order
- We need to find if ANY substring of `s2` with length `len(s1)` has the same character frequencies as `s1`
- This is a classic **Sliding Window** problem with **Frequency Matching**

### 🎯 Algorithm Strategy
1. **Frequency Counting**: Use arrays to count character frequencies (faster than hashmaps for lowercase letters)
2. **Sliding Window**: Maintain a window of size `len(s1)` and slide it across `s2`
3. **Optimization**: Instead of recalculating frequencies, update incrementally

---

## 💻 Optimized Solution

```python
class Solution:
    def checkInclusion(self, pattern: str, text: str) -> bool:
        pattern_length = len(pattern)
        text_length = len(text)
        
        # Edge case: pattern longer than text
        if pattern_length > text_length:
            return False
        
        # Frequency arrays for 26 lowercase letters (a-z)
        pattern_freq = [0] * 26  # Target frequency we want to match
        window_freq = [0] * 26   # Current sliding window frequency
        
        # Build initial frequency arrays for first window
        for char_index in range(pattern_length):
            # Count frequency in pattern
            pattern_char_code = ord(pattern[char_index]) - ord('a')
            pattern_freq[pattern_char_code] += 1
            
            # Count frequency in first window of text
            text_char_code = ord(text[char_index]) - ord('a')
            window_freq[text_char_code] += 1
        
        # Check if first window matches
        if pattern_freq == window_freq:
            return True
        
        # Slide the window across remaining text
        for right_index in range(pattern_length, text_length):
            # Add new character entering the window (right side)
            new_char_code = ord(text[right_index]) - ord('a')
            window_freq[new_char_code] += 1
            
            # Remove character leaving the window (left side)
            left_index = right_index - pattern_length
            old_char_code = ord(text[left_index]) - ord('a')
            window_freq[old_char_code] -= 1
            
            # Check if current window matches pattern frequency
            if pattern_freq == window_freq:
                return True
        
        return False
```

---

## 🎤 Interview Communication Strategy

### 🗣️ Step-by-Step Walkthrough

#### **Step 1: Problem Clarification (30 seconds)**
> "Let me confirm my understanding: We need to check if string s2 contains any anagram of s1 as a substring. An anagram means same characters with same frequencies but possibly different order, correct?"

#### **Step 2: Approach Explanation (1-2 minutes)**
> "I'll use a sliding window approach with frequency counting:
> 1. First, I'll count character frequencies in s1 - this is our target
> 2. Then I'll use a sliding window of size len(s1) on s2
> 3. For each window position, I'll compare frequencies
> 4. Instead of recalculating frequencies each time, I'll update incrementally by adding the new character and removing the old one"

#### **Step 3: Edge Cases Discussion**
> "Key edge cases to consider:
> - s1 longer than s2: impossible to find permutation
> - s1 and s2 have same length: direct frequency comparison
> - Empty strings: based on problem constraints, won't occur"

#### **Step 4: Complexity Analysis**
> "Time Complexity: O(n) where n is length of s2
> - We traverse s2 once with sliding window
> - Each frequency comparison is O(26) = O(1) for lowercase letters
> 
> Space Complexity: O(1) 
> - Fixed size arrays of 26 elements regardless of input size"

---

## 🔍 Key Interview Observations to Share

### 🎯 **Optimization Insights**
1. **Array vs HashMap**: "I'm using arrays instead of hashmaps because we only have 26 lowercase letters, making array access O(1) and more cache-friendly"

2. **Incremental Updates**: "Instead of recalculating window frequencies each time, I update incrementally - this reduces complexity from O(n×m) to O(n)"

3. **Early Termination**: "I check the first window separately to potentially return early"

### 🚨 **Common Pitfalls to Mention**
- "I need to be careful with array indexing: `ord(char) - ord('a')` converts 'a' to 0, 'b' to 1, etc."
- "The sliding window update requires both adding new character AND removing old character"
- "Window size must remain constant at `len(s1)`"

---

## 🧪 Test Case Walkthrough

```python
# Example: s1 = "ab", s2 = "eidbaooo"
# Initial state:
pattern_freq = [1, 1, 0, 0, ...] # 'a'=1, 'b'=1
window_freq  = [0, 0, 0, 0, ...]

# Window 1: "ei" - frequencies don't match
# Window 2: "id" - frequencies don't match  
# Window 3: "db" - frequencies don't match
# Window 4: "ba" - frequencies match! Return True
```

---

## 🏆 Interview Success Tips

### ✅ **What Interviewers Look For**
1. **Clear Problem Understanding**: Correctly identify this as sliding window + frequency matching
2. **Optimal Approach**: Choose O(n) solution over brute force O(n×m×26)
3. **Clean Code**: Well-named variables, proper edge case handling
4. **Communication**: Explain your thought process clearly
5. **Testing**: Walk through examples and edge cases

### 🎯 **Bonus Points**
- Mention alternative approaches (sorting-based, but less efficient)
- Discuss how this extends to other character sets (Unicode, etc.)
- Show awareness of memory vs time tradeoffs

### ⚠️ **Red Flags to Avoid**
- Generating all permutations of s1 (exponential complexity)
- Nested loops without optimization
- Forgetting edge cases
- Poor variable naming or lack of comments

---

## 🚀 Final Interview Confidence Booster

**Remember:** This problem tests your ability to:
- Recognize sliding window patterns
- Optimize with frequency arrays
- Handle string manipulation efficiently
- Communicate complex algorithms clearly


---

## 🎯 Key Observations to Share During Interview

- A permutation check can be reduced to **frequency comparison**.
- By using **sliding window** of length `len(s1)` across `s2`, we can maintain an updated frequency count.
- Use fixed-length frequency arrays (size 26) for lowercase characters for O(1) comparison.
- **Edge case**: If `len(s1) > len(s2)`, no solution is possible.

---

## 🧠 Interviewer Expectations

When explaining this solution in an interview, be sure to:

1. **Clarify Your Approach**

   * “I’m using a sliding window approach of size equal to s1, and comparing frequency maps.”

2. **Justify Data Structures**

   * “Using fixed-size lists of 26 for constant-time frequency comparison.”

3. **Explain Edge Cases**

   * “If the pattern is longer than the text, return False immediately.”

4. **Complexity Analysis**

   * Time: `O(n)` where n = length of `s2`, since each character is visited once.
   * Space: `O(1)` because the frequency arrays are fixed size (26).

---



## 🧠 Final Notes

* This is a great problem to demonstrate understanding of **frequency counting**, **window manipulation**, and **optimizing comparisons**.
* Avoid sorting-based permutation checks – they are slower (O(n log n)).

---

## 📌 Related Patterns

* 🔁 [Sliding Window Technique](https://leetcode.com/tag/sliding-window/)
* 🔠 [Character Count Problems](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

---

**Practice saying:** *"This is a classic sliding window problem where we maintain character frequencies and slide efficiently across the text to find matching anagram patterns."*
