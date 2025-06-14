
---

# 🪟 Sliding Window Patterns — Greg Hogg's Playlist Summary

This document summarizes common sliding window problems and their solutions, as covered in [Greg Hogg's YouTube playlist on Sliding Window Patterns](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W). It is designed to help you efficiently study the technique and apply it across different problem types.

---

## 📌 1. [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/description/)

**Video Explanation**: [Sliding Window: Max Consecutive Ones III - Greg Hogg](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=1)

### 💡 Problem Type: Fixed-Size/Expanding Window

You are allowed to flip at most `k` zeros to ones to get the **maximum consecutive ones** in the array.

### 🧠 Pattern Used:

* Keep track of the number of zeros in the current window.
* Shrink the window when zeros exceed `k`.

---

## 📌 2. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**Video Explanation**: [Sliding Window: Longest Substring Without Repeating Characters - Greg Hogg](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=2)

### 💡 Problem Type: Variable-Size Window

Find the length of the **longest substring** without any repeated characters.

### 🧠 Pattern Used:

* Use a `Set` or `Map` to track character positions.
* Move the left pointer when a repeat is found.

---

## 📌 3. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

**Video Explanation**: [Sliding Window: Longest Repeating Character Replacement - Greg Hogg](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=3)

### 💡 Problem Type: Variable-Size Window

Change at most `k` characters to make the longest possible substring of one repeating character.

### 🧠 Pattern Used:

* Maintain frequency count of characters.
* Shrink the window if the replacements needed exceed `k`.

---

## 📌 4. [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

**Video Explanation**: [Sliding Window: Minimum Size Subarray Sum - Greg Hogg](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=4)

### 💡 Problem Type: Shrinking Window

Find the smallest length of a contiguous subarray with a sum ≥ target.

### 🧠 Pattern Used:

* Expand the window until the sum ≥ target.
* Shrink from the left to find the minimum length.

---

## 📌 5. [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

**Video Explanation**: [Sliding Window: Maximum Average Subarray I - Greg Hogg](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=5)

### 💡 Problem Type: Fixed-Size Window

Find the subarray of size `k` with the **maximum average**.

### 🧠 Pattern Used:

* Maintain a running sum of the last `k` elements.
* Use a fixed sliding window to track the max sum.

---

## 📌 6. [Permutation in String](https://leetcode.com/problems/permutation-in-string/)

**Video Explanation**: [Sliding Window: Permutation in String - Greg Hogg](https://www.youtube.com/watch?v=FCbOzdHKW18&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W&index=6)

### 💡 Problem Type: Sliding Window + Frequency Matching

Check if one string contains a permutation of another.

### 🧠 Pattern Used:

* Use two frequency maps (or arrays) for comparison.
* Slide the window across the second string and check for matches.

---

## 🎓 Full Playlist

Watch all videos here:
▶️ **[Sliding Window Patterns by Greg Hogg (YouTube Playlist)](https://www.youtube.com/watch?v=HsGKI02yw6M&list=PLKYEe2WisBTFZH-p9jgAOwtHy9_LGI28W)**

---

