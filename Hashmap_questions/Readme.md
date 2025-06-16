# Hashmap Interview Questions – Greg Hogg Playlist

---

## Table of Contents

1. [Jewels and Stones](#jewels-and-stones)
2. [Ransom Note](#ransom-note)
3. [Contains Duplicate](#contains-duplicate)
4. [Maximum Number of Balloons](#maximum-number-of-balloons)
5. [Valid Sudoku](#valid-sudoku)
6. [Two Sum](#two-sum)
7. [Valid Anagram](#valid-anagram)
8. [Group Anagrams](#group-anagrams)
9. [Longest Consecutive Sequence](#longest-consecutive-sequence)
10. [Majority Element](#majority-element)
11. [Visual Summary Table](#visual-summary-table)
12. [Hashmap Interview Tips](#hashmap-interview-tips)

---

## 1. Jewels and Stones

- **Description:** Count how many characters in a string (stones) are also in another string (jewels) using a set for fast lookup.
- **LeetCode:** [LeetCode 771](https://leetcode.com/problems/jewels-and-stones/)
- **Companies:** Amazon, Google, Facebook[1][2].

---

## 2. Ransom Note

- **Description:** Determine if a ransom note can be constructed from the letters in a magazine using a hashmap to count character frequencies.
- **LeetCode:** [LeetCode 383](https://leetcode.com/problems/ransom-note/)
- **Companies:** Amazon, Microsoft, Facebook[2][3].

---

## 3. Contains Duplicate

- **Description:** Check if an array contains any duplicates by leveraging a set for O(1) lookups.
- **LeetCode:** [LeetCode 217](https://leetcode.com/problems/contains-duplicate/)
- **Companies:** Amazon, Google, Microsoft[1][2].

---

## 4. Maximum Number of Balloons

- **Description:** Given a string, return the maximum number of times the word “balloon” can be formed using its letters, tracked with a hashmap.
- **LeetCode:** [LeetCode 1189](https://leetcode.com/problems/maximum-number-of-balloons/)
- **Companies:** Amazon, Google[2][3].

---

## 5. Valid Sudoku

- **Description:** Validate a 9x9 Sudoku board using hashmaps/sets to ensure no repetitions in rows, columns, and sub-grids.
- **LeetCode:** [LeetCode 36](https://leetcode.com/problems/valid-sudoku/)
- **Companies:** Google, Microsoft, Apple[2][3].

---

## 6. Two Sum

- **Description:** Find indices of two numbers in an array that sum to a target using a hashmap for O(1) complement lookups.
- **LeetCode:** [LeetCode 1](https://leetcode.com/problems/two-sum/)
- **Companies:** Amazon, Google, Facebook[1][2].

---

## 7. Valid Anagram

- **Description:** Check if two strings are anagrams by comparing their character counts using hashmaps.
- **LeetCode:** [LeetCode 242](https://leetcode.com/problems/valid-anagram/)
- **Companies:** Facebook, Amazon, Microsoft[2][3].

---

## 8. Group Anagrams

- **Description:** Group strings that are anagrams of each other using a hashmap with sorted strings as keys.
- **LeetCode:** [LeetCode 49](https://leetcode.com/problems/group-anagrams/)
- **Companies:** Amazon, Google, Facebook[2][3].

---

## 9. Longest Consecutive Sequence

- **Description:** Find the length of the longest consecutive elements sequence in an unsorted array using a set for O(1) lookups.
- **LeetCode:** [LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)
- **Companies:** Google, Amazon, Microsoft[1][2].

---

## 10. Majority Element

- **Description:** Find the element that appears more than ⌊n/2⌋ times in an array using a hashmap for counting.
- **LeetCode:** [LeetCode 169](https://leetcode.com/problems/majority-element/)
- **Companies:** Amazon, Microsoft, Apple[2][3].

---

## 11. Visual Summary Table

| Problem                        | LeetCode Link                                                                      | Companies                         |
|---------------------------------|------------------------------------------------------------------------------------|-----------------------------------|
| Jewels and Stones               | [LeetCode 771](https://leetcode.com/problems/jewels-and-stones/)                   | Amazon, Google, Facebook[1][2]    |
| Ransom Note                     | [LeetCode 383](https://leetcode.com/problems/ransom-note/)                         | Amazon, Microsoft, Facebook[2][3] |
| Contains Duplicate              | [LeetCode 217](https://leetcode.com/problems/contains-duplicate/)                  | Amazon, Google, Microsoft[1][2]   |
| Maximum Number of Balloons      | [LeetCode 1189](https://leetcode.com/problems/maximum-number-of-balloons/)         | Amazon, Google[2][3]              |
| Valid Sudoku                    | [LeetCode 36](https://leetcode.com/problems/valid-sudoku/)                         | Google, Microsoft, Apple[2][3]    |
| Two Sum                         | [LeetCode 1](https://leetcode.com/problems/two-sum/)                               | Amazon, Google, Facebook[1][2]    |
| Valid Anagram                   | [LeetCode 242](https://leetcode.com/problems/valid-anagram/)                       | Facebook, Amazon, Microsoft[2][3] |
| Group Anagrams                  | [LeetCode 49](https://leetcode.com/problems/group-anagrams/)                       | Amazon, Google, Facebook[2][3]    |
| Longest Consecutive Sequence    | [LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)        | Google, Amazon, Microsoft[1][2]   |
| Majority Element                | [LeetCode 169](https://leetcode.com/problems/majority-element/)                    | Amazon, Microsoft, Apple[2][3]    |

---

## 12. Hashmap Interview Tips

- **When to Use Hashmaps:** Use hashmaps for fast lookups, counting, and grouping data by keys[2][3].
- **Common Pitfalls:** Handle duplicate keys, hash collisions, and be aware of space complexity[2][4].
- **Hashmap Operations:** Most operations (insert, delete, search) are O(1) on average, but can degrade with poor hash functions[2][3].
- **Practice:** Focus on problems involving counting, grouping, and set membership.

---


Here's an improved version of your **Hashmap Interview Questions – Greg Hogg Playlist** document, redesigned for **maximum interview preparation value**. This includes:

* 🚀 Crisper descriptions
* ✅ Realistic interview expectations
* 💡 Key observations per problem
* 🧠 Pattern recognition prompts
* 🛠 Clean structure for fast skimming

---

````markdown
# 📘 Hashmap Interview Questions – Greg Hogg Playlist

> 📺 [Watch the Full Playlist](https://www.youtube.com/playlist?list=PLKYEe2WisBTFvzq98lMIhWfXH3KMTXZv0)  
> 🎯 Master hash-based problems asked at FAANG and top startups

---

## 📑 Table of Contents

1. [Jewels and Stones](#1-jewels-and-stones)
2. [Ransom Note](#2-ransom-note)
3. [Contains Duplicate](#3-contains-duplicate)
4. [Maximum Number of Balloons](#4-maximum-number-of-balloons)
5. [Valid Sudoku](#5-valid-sudoku)
6. [Two Sum](#6-two-sum)
7. [Valid Anagram](#7-valid-anagram)
8. [Group Anagrams](#8-group-anagrams)
9. [Longest Consecutive Sequence](#9-longest-consecutive-sequence)
10. [Majority Element](#10-majority-element)
11. [Visual Summary Table](#11-visual-summary-table)
12. [Interview Tips & Expectations](#12-interview-tips--expectations)

---

## 1. Jewels and Stones

- **🧠 Concept:** Membership check using a `set`.
- **Problem:** Count how many characters in `stones` are also in `jewels`.
- **LC:** [771. Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)
- **Companies:** Amazon, Google, Facebook
- **Code Insight:**
```python
def numJewelsInStones(jewels, stones):
    jewel_set = set(jewels)
    return sum(c in jewel_set for c in stones)
````

---

## 2. Ransom Note

* **🧠 Concept:** Character frequency using a hashmap.
* **Problem:** Can `ransomNote` be built using letters from `magazine`?
* **LC:** [383. Ransom Note](https://leetcode.com/problems/ransom-note/)
* **Companies:** Amazon, Microsoft
* **Code Insight:**

```python
from collections import Counter
def canConstruct(ransomNote, magazine):
    return not (Counter(ransomNote) - Counter(magazine))
```

---

## 3. Contains Duplicate

* **🧠 Concept:** Detect duplicates using a `set`.
* **LC:** [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
* **Companies:** Amazon, Google
* **Code Insight:**

```python
def containsDuplicate(nums):
    return len(nums) != len(set(nums))
```

---

## 4. Maximum Number of Balloons

* **🧠 Concept:** Frequency counting, handle repeating characters (`'l'`, `'o'`).
* **LC:** [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)
* **Companies:** Amazon
* **Code Insight:**

```python
from collections import Counter
def maxNumberOfBalloons(text):
    count = Counter(text)
    return min(count['b'], count['a'], count['l']//2, count['o']//2, count['n'])
```

---

## 5. Valid Sudoku

* **🧠 Concept:** Hashing rows, columns, boxes with (row, col) keys.
* **LC:** [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)
* **Companies:** Google, Apple
* **Code Insight:**

```python
def isValidSudoku(board):
    seen = set()
    for i in range(9):
        for j in range(9):
            val = board[i][j]
            if val != '.':
                if (i, val) in seen or (val, j) in seen or (i//3, j//3, val) in seen:
                    return False
                seen |= {(i, val), (val, j), (i//3, j//3, val)}
    return True
```

---

## 6. Two Sum

* **🧠 Concept:** Lookup complement using hashmap.
* **LC:** [1. Two Sum](https://leetcode.com/problems/two-sum/)
* **Companies:** Facebook, Amazon
* **Code Insight:**

```python
def twoSum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        if target - num in seen:
            return [seen[target - num], i]
        seen[num] = i
```

---

## 7. Valid Anagram

* **🧠 Concept:** Compare two frequency maps.
* **LC:** [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
* **Companies:** Facebook
* **Code Insight:**

```python
from collections import Counter
def isAnagram(s, t):
    return Counter(s) == Counter(t)
```

---

## 8. Group Anagrams

* **🧠 Concept:** Group by sorted string as hash key.
* **LC:** [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
* **Companies:** Amazon, Google
* **Code Insight:**

```python
from collections import defaultdict
def groupAnagrams(strs):
    groups = defaultdict(list)
    for word in strs:
        groups[tuple(sorted(word))].append(word)
    return list(groups.values())
```

---

## 9. Longest Consecutive Sequence

* **🧠 Concept:** Use a set and expand from each number.
* **LC:** [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
* **Companies:** Google, Amazon
* **Code Insight:**

```python
def longestConsecutive(nums):
    num_set = set(nums)
    longest = 0
    for n in num_set:
        if n - 1 not in num_set:
            length = 1
            while n + length in num_set:
                length += 1
            longest = max(longest, length)
    return longest
```

---

## 10. Majority Element

* **🧠 Concept:** Count frequency > n//2.
* **LC:** [169. Majority Element](https://leetcode.com/problems/majority-element/)
* **Companies:** Microsoft, Apple
* **Code Insight:**

```python
def majorityElement(nums):
    count = {}
    for n in nums:
        count[n] = count.get(n, 0) + 1
        if count[n] > len(nums) // 2:
            return n
```

---

## 11. Visual Summary Table

| Problem                      | LeetCode Link                                                      | Companies                |
| ---------------------------- | ------------------------------------------------------------------ | ------------------------ |
| Jewels and Stones            | [771](https://leetcode.com/problems/jewels-and-stones/)            | Amazon, Google, Facebook |
| Ransom Note                  | [383](https://leetcode.com/problems/ransom-note/)                  | Amazon, Microsoft        |
| Contains Duplicate           | [217](https://leetcode.com/problems/contains-duplicate/)           | Amazon, Google           |
| Maximum Number of Balloons   | [1189](https://leetcode.com/problems/maximum-number-of-balloons/)  | Amazon                   |
| Valid Sudoku                 | [36](https://leetcode.com/problems/valid-sudoku/)                  | Google, Apple            |
| Two Sum                      | [1](https://leetcode.com/problems/two-sum/)                        | Amazon, Facebook         |
| Valid Anagram                | [242](https://leetcode.com/problems/valid-anagram/)                | Facebook                 |
| Group Anagrams               | [49](https://leetcode.com/problems/group-anagrams/)                | Amazon, Google           |
| Longest Consecutive Sequence | [128](https://leetcode.com/problems/longest-consecutive-sequence/) | Google, Amazon           |
| Majority Element             | [169](https://leetcode.com/problems/majority-element/)             | Microsoft, Apple         |

---

## 12. Interview Tips & Expectations

### ✅ What to Explain in Interviews:

* **Why a hashmap?** O(1) access and perfect for counting/frequency/membership.
* **Space Complexity?** Hashmaps can grow large, discuss trade-offs.
* **Alternatives?** Set, sorting, brute-force — explain why they’re suboptimal.
* **Edge Cases:** Empty input, duplicate data, non-ASCII characters.

### 🎯 Behavioral Communication:

* Talk through **brute-force first**, then optimize with hashmap.
* Use examples to **simulate the algorithm** in real time.
* Highlight **time vs space trade-offs**.
* Ask: “Can I assume all inputs are lowercase?”, “Are there duplicates?”

---

> ✨ Mastering these hashmap patterns gives you a huge edge in system design, dynamic programming, and real-time applications like spell checkers, event counters, and recommendation systems.


---
For detailed explanations and code solutions, refer to Greg Hogg’s [YouTube playlist](https://www.youtube.com/playlist?list=PLKYEe2WisBTFvzq98lMIhWfXH3KMTXZv0)[1][3].

[1] https://www.youtube.com/watch?v=JlbhUhTec-s
[2] https://www.finalroundai.com/blog/hashmap-interview-questions
[3] https://interviewnoodle.com/path-to-conquer-leetcode-easy-part-1-hashmap-and-two-sums-baba53e36496
[4] https://in.indeed.com/career-advice/interviewing/hashmap-interview-questions
[5] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/55174337/7dcae6d4-947c-467c-b40f-9895320576cf/1-Array-String-Questions-YouTube.pdf
[6] https://pplx-res.cloudinary.com/image/private/user_uploads/55174337/dbc4e402-3a40-40ce-a414-7e543b0a73ab/Screenshot-2025-06-17-at-12.30.40-AM.jpg
[7] https://www.youtube.com/playlist?list=PLKYEe2WisBTFvzq98lMIhWfXH3KMTXZv0
[8] https://hackage.haskell.org/package/vector/reverse/flat
[9] https://www.youtube.com/c/GregHogg/playlists
[10] https://www.youtube.com/watch?v=iZyxNEBpqFY
[11] https://www.youtube.com/watch?v=aRE7Nxb3Qfs
[12] https://www.youtube.com/watch?v=njS4SjIBMAU
