# Hashmaps Revised Important Questions
## LC 49 Group Anagrams – Interview Guide

---

## 🧩 Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

---

## ⚡ Interview Ready Optimized Solution (Using Character Count Signature)

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagram_groups = defaultdict(list)

        for word in strs:
            char_count = [0] * 26  # Frequency of a-z
            for char in word:
                char_count[ord(char) - ord('a')] += 1
            key = tuple(char_count)
            anagram_groups[key].append(word)

        return list(anagram_groups.values())
```

### ⏱ Time Complexity

* **O(n × m)** — counting character frequencies per word

### 📦 Space Complexity

* **O(n × m)** — storing words and 26-length keys

---

### 🔍 Optimized – Step-by-Step Example

**Input:**

```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```

| Step | Word  | Char Count Tuple Key    | Dictionary State                      |
| ---- | ----- | ----------------------- | ------------------------------------- |
| 1    | "eat" | tuple for `'a','e','t'` | `{ key1: ["eat"] }`                   |
| 2    | "tea" | same as above           | `{ key1: ["eat", "tea"] }`            |
| 3    | "tan" | tuple for `'a','n','t'` | `{ key1: [...], key2: ["tan"] }`      |
| 4    | "ate" | same as "eat"           | `{ key1: [..., "ate"], key2: [...] }` |
| 5    | "nat" | same as "tan"           | `{ key1: [...], key2: [..., "nat"] }` |
| 6    | "bat" | tuple for `'a','b','t'` | `{ ..., key3: ["bat"] }`              |

**Output:**

```python
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

---
---

## ✅ Brute Force Solution (Using Sorted Strings)

```python
from collections import defaultdict
from typing import List

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagram_groups = defaultdict(list)

        for word in strs:
            key = ''.join(sorted(word))  # Sorting as the signature
            anagram_groups[key].append(word)

        return list(anagram_groups.values())
````

### ⏱ Time Complexity

* **O(n × m log m)** — `n` words, each of length up to `m` (sorted per word)

### 📦 Space Complexity

* **O(n × m)** — storing all words in groups + keys

---

### 🔍 Brute Force – Step-by-Step Example

**Input:**

```python
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
```

| Step | Word  | Sorted Key | Dictionary State                        |
| ---- | ----- | ---------- | --------------------------------------- |
| 1    | "eat" | `"aet"`    | `{ "aet": ["eat"] }`                    |
| 2    | "tea" | `"aet"`    | `{ "aet": ["eat", "tea"] }`             |
| 3    | "tan" | `"ant"`    | `{ "aet": [...], "ant": ["tan"] }`      |
| 4    | "ate" | `"aet"`    | `{ "aet": [..., "ate"], "ant": [...] }` |
| 5    | "nat" | `"ant"`    | `{ "aet": [...], "ant": [..., "nat"] }` |
| 6    | "bat" | `"abt"`    | `{ ..., "abt": ["bat"] }`               |

**Output:**

```python
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```
---

## 💬 Interview Notes

### ✅ Talk Through These Points

* Why sorting works (grouped by normalized form)
* Why counting letters is better (fixed size, faster)
* Tradeoffs between readability (brute) vs performance (optimized)
* Handle edge cases: `[""]`, `["a"]`, same letters with different frequency

---

## 🔁 Final Comparison

| Criteria         | Brute Force    | Optimized                 |
| ---------------- | -------------- | ------------------------- |
| Key Used         | Sorted string  | Character count tuple     |
| Time Complexity  | O(n × m log m) | O(n × m)                  |
| Space Complexity | O(n × m)       | O(n × m)                  |
| Readability      | ✅ Easy         | ⚠ Slightly more complex   |
| Performance      | ⚠ Slower       | ✅ Faster for long strings |

---

