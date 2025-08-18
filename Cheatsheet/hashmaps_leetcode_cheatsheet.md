# Hashmaps in Python: LeetCode Cheatsheet

## Basics

- **Definition**  
  ```python
  mydict = {"Canada": ["Calgary", "Vancouver"], "USA": ["Chicago", "Seattle"], "England": ["London", "Manchester"]}
  ```
- **Initialization**
  ```python
  from collections import defaultdict, Counter
  mydict = defaultdict(list)  # auto-initializes missing keys with []
  counter = Counter()         # frequency counter
  ```
- **Access / Update Values**
  ```python
  mydict["Canada"].append("Toronto")
  mydict.get("USA", [])      # returns value or [] if missing
  ```

## Traversal

- **Keys:**  
  `for k in mydict.keys():`
- **Values:**  
  `for v in mydict.values():`
- **Key-Value pairs:**  
  `for k, v in mydict.items():`

## Common Methods

| Method                   | Description               | Example                             |
|--------------------------|---------------------------|-------------------------------------|
| get(key, default)        | Safe access with default  | mydict.get("Spain", [])             |
| setdefault(key, default) | Insert if missing         | mydict.setdefault("Spain", [])      |
| pop(key, default)        | Remove by key             | mydict.pop("USA")                   |
| keys(), values(), items()| Iteration utilities       | mydict.items()                      |
| update(dict2)            | Merge another dict        | mydict.update(otherdict)            |
| clear()                  | Remove all items          | mydict.clear()                      |

***

## Patterns & LeetCode Scenarios

### 1. **Group Anagrams**
```python
def groupAnagrams(strs):
    ans = defaultdict(list)
    for s in strs:
        key = tuple(sorted(s))
        ans[key].append(s)
    return list(ans.values())
```

### 2. **Longest Substring Without Repeating Characters**
```python
def lengthOfLongestSubstring(s):
    index_map = {}
    start = max_len = 0
    for i, c in enumerate(s):
        if c in index_map and index_map[c] >= start:
            start = index_map[c] + 1
        index_map[c] = i
        max_len = max(max_len, i - start + 1)
    return max_len
```

### 3. **Two Sum**
```python
def twoSum(nums, target):
    lookup = {}
    for i, n in enumerate(nums):
        if target - n in lookup:
            return [lookup[target - n], i]
        lookup[n] = i
```

### 4. **Top K Frequent Elements**
```python
def topKFrequent(nums, k):
    count = Counter(nums)
    return [x[0] for x in count.most_common(k)]
```

### 5. **Check for Duplicates**
```python
def containsDuplicate(nums):
    seen = set()
    for n in nums:
        if n in seen:
            return True
        seen.add(n)
    return False
```

### 6. **Minimum Window Substring**
```python
def minWindow(s, t):
    need = Counter(t)
    have = Counter()
    left = right = formed = 0
    res = (float('inf'), 0, 0)
    while right int]
```
---

| Use Case / Pattern             | Python Structure                     | Equivalent Concept (Interview Speak) | Example Code Snippet                   |
| ------------------------------ | ------------------------------------ | ------------------------------------ | -------------------------------------- |
| **Fast lookups by value/key**  | `dict` / `set`                       | Hashmap / Hashset                    | `if x in seen: ...`                    |
| **Track last seen indices**    | `dict[str, int]`                     | `hashmap<char → last_index>`         | `last_seen[c] = i`                     |
| **Count frequencies**          | `Counter`, `defaultdict(int)`        | Frequency map                        | `count[num] += 1`                      |
| **Group by key / category**    | `defaultdict(list)`                  | Bucketing via hashmap                | `groups[key].append(val)`              |
| **Check duplicates**           | `set`                                | Hashset                              | `if n in seen: return True`            |
| **Keep insertion order**       | `dict` (≥3.7), `OrderedDict`         | Ordered Hashmap                      | `for k,v in mydict.items()`            |
| **LRU Cache / MRU patterns**   | `OrderedDict`, `functools.lru_cache` | Cache hashmap                        | `cache.move_to_end(key)`               |
| **Sliding window with counts** | `dict` / `Counter`                   | Window hashmap                       | `while count[ch] > need[ch]: ...`      |
| **Prefix sums with lookup**    | `dict[int, int]`                     | Prefix sum hashmap                   | `prefix_map[sum] = idx`                |
| **Mapping entities**           | `dict[key_type, val_type]`           | Lookup table                         | `country_to_capital["France"]="Paris"` |

---

| Method                    | Use Case            | Example                      |
| ------------------------- | ------------------- | ---------------------------- |
| `.get(key, default)`      | Safe lookup         | `mydict.get("Spain", [])`    |
| `.setdefault(key, value)` | Insert if missing   | `mydict.setdefault("x", 0)`  |
| `.pop(key, default)`      | Remove by key       | `mydict.pop("USA")`          |
| `.update(dict2)`          | Merge another dict  | `mydict.update(otherdict)`   |
| `.keys(), .values()`      | Iteration utilities | `for k,v in mydict.items():` |
| `.clear()`                | Empty dict          | `mydict.clear()`             |

***
---

## Notes

- For default values, use `defaultdict`.
- For missing keys, always use `.get()` or `setdefault()`.
- Hashmaps are O(1) average for lookups and updates.
- Prefer `Counter` and `defaultdict` from `collections` for specialized counting logic.

***
