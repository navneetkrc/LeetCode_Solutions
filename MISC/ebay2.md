# 📄 Interview Quick Codes — Python (with Complexity)

## 1️⃣ Two Sum

**LeetCode #1**

```python
def twoSum(nums, target):
    hashmap = {}
    for i, num in enumerate(nums):
        if target - num in hashmap:
            return [hashmap[target - num], i]
        hashmap[num] = i

# Time: O(n) — single pass over nums
# Space: O(n) — hashmap stores up to n elements
```

---

## 2️⃣ Set Matrix Zeroes

**LeetCode #73**

**Must discuss first approach with O(m+n) space, then optimize to give final 2nd approach with O(1) constant space**

```python
def setZeroes(matrix):
    rows, cols = set(), set()
    for i in range(len(matrix)):
        for j in range(len(matrix[0])):
            if matrix[i][j] == 0:
                rows.add(i)
                cols.add(j)
    for i in range(len(matrix)):
        for j in range(len(matrix[0])):
            if i in rows or j in cols:
                matrix[i][j] = 0

# Time: O(m × n) — scan + update entire matrix
# Space: O(m + n) — to store row & column indices
```
---

```python
class Solution:
    def setZeroes(self, matrix: list[list[int]]) -> None:
        rows, cols = len(matrix), len(matrix[0])

        # Check if first row has any zeros
        first_row_has_zero = any(matrix[0][j] == 0 for j in range(cols))

        # Check if first column has any zeros
        first_col_has_zero = any(matrix[i][0] == 0 for i in range(rows))

        # Mark rows and columns that should be zeroed
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][j] == 0:
                    matrix[i][0] = 0  # mark row
                    matrix[0][j] = 0  # mark column

        # Set cells to zero based on marks
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0

        # Zero out the first row if needed
        if first_row_has_zero:
            for j in range(cols):
                matrix[0][j] = 0

        # Zero out the first column if needed
        if first_col_has_zero:
            for i in range(rows):
                matrix[i][0] = 0
# Time: O(m × n) — scans entire matrix once for marking, once for updating
# Space: O(1) — in-place, uses first row & col as storage
```

---
## 3️⃣ Add Two Numbers (Linked Lists)

**LeetCode #2**

```python
class ListNode:
    def __init__(self, val=0, next=None): self.val, self.next = val, next

def addTwoNumbers(l1, l2):
    dummy = curr = ListNode()
    carry = 0
    while l1 or l2 or carry:
        v1, v2 = (l1.val if l1 else 0), (l2.val if l2 else 0)
        carry, val = divmod(v1 + v2 + carry, 10)
        curr.next = ListNode(val)
        curr = curr.next
        l1, l2 = (l1.next if l1 else None), (l2.next if l2 else None)
    return dummy.next

# Time: O(max(m, n)) — iterate over both lists
# Space: O(max(m, n)) — for output list (excluding input storage)
```

---

## 4️⃣ Binary Tree Level Order Traversal

**LeetCode #102**

```python
from collections import deque

def levelOrder(root):
    if not root: return []
    result, dq = [], deque([root])
    while dq:
        level = []
        for _ in range(len(dq)):
            node = dq.popleft()
            level.append(node.val)
            if node.left: dq.append(node.left)
            if node.right: dq.append(node.right)
        result.append(level)
    return result

# Time: O(n) — visit each node once
# Space: O(n) — queue can store up to n/2 nodes in last level
```

---

## 5️⃣ Recover Binary Search Tree

**LeetCode #99**

```python
def recoverTree(root):
    first = second = prev = None
    def inorder(node):
        nonlocal first, second, prev
        if not node: return
        inorder(node.left)
        if prev and prev.val > node.val:
            if not first: first = prev
            second = node
        prev = node
        inorder(node.right)
    inorder(root)
    first.val, second.val = second.val, first.val

# Time: O(n) — inorder traversal visits all nodes
# Space: O(h) — recursion stack height h (O(log n) for balanced, O(n) worst)
```

---

## 6️⃣ Search in Rotated Sorted Array

**LeetCode #33**

```python
def search(nums, target):
    l, r = 0, len(nums)-1
    while l <= r:
        mid = (l+r)//2
        if nums[mid] == target: return mid
        if nums[l] <= nums[mid]:
            if nums[l] <= target < nums[mid]: r = mid - 1
            else: l = mid + 1
        else:
            if nums[mid] < target <= nums[r]: l = mid + 1
            else: r = mid - 1
    return -1

# Time: O(log n) — binary search halves search space
# Space: O(1) — constant space
```

---

## 7️⃣ Palindrome Permutation

**LeetCode #266**

```python
from collections import Counter

def canPermutePalindrome(s):
    return sum(v % 2 for v in Counter(s).values()) <= 1

# Time: O(n) — count all characters
# Space: O(1) — at most 26/128 counts depending on char set
```

---

## 8️⃣ Copy List with Random Pointer

**LeetCode #138**

```python
class Node:
    def __init__(self, val=0, next=None, random=None):
        self.val, self.next, self.random = val, next, random

def copyRandomList(head):
    if not head: return None
    old_to_new = {}
    cur = head
    while cur:
        old_to_new[cur] = Node(cur.val)
        cur = cur.next
    cur = head
    while cur:
        old_to_new[cur].next = old_to_new.get(cur.next)
        old_to_new[cur].random = old_to_new.get(cur.random)
        cur = cur.next
    return old_to_new[head]

# Time: O(n) — iterate twice over list
# Space: O(n) — hashmap stores mapping of old to new nodes
```

---



# Interview-Ready Coding Solutions

## 1. Two Sum (LeetCode #1)
**Problem:** Find two numbers in array that add up to target
```python
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

# Time: O(n), Space: O(n)
# Example: nums=[2,7,11,15], target=9 → [0,1]
```

## 2. Set Matrix Zeroes (LeetCode #73)
**Problem:** Set entire row/col to 0 if any element is 0
```python
def set_zeroes(matrix):
    m, n = len(matrix), len(matrix[0])
    first_row_zero = any(matrix[0][j] == 0 for j in range(n))
    first_col_zero = any(matrix[i][0] == 0 for i in range(m))
    
    # Mark zeros using first row/col
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[0][j] = matrix[i][0] = 0
    
    # Set zeros based on marks
    for i in range(1, m):
        for j in range(1, n):
            if matrix[0][j] == 0 or matrix[i][0] == 0:
                matrix[i][j] = 0
    
    # Handle first row/col
    if first_row_zero:
        for j in range(n):
            matrix[0][j] = 0
    if first_col_zero:
        for i in range(m):
            matrix[i][0] = 0

# Time: O(m*n), Space: O(1)
```

## 3. Add Two Numbers (LeetCode #2)
**Problem:** Add two numbers represented as linked lists
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def add_two_numbers(l1, l2):
    dummy = ListNode(0)
    current = dummy
    carry = 0
    
    while l1 or l2 or carry:
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0
        
        total = val1 + val2 + carry
        carry = total // 10
        current.next = ListNode(total % 10)
        
        current = current.next
        l1 = l1.next if l1 else None
        l2 = l2.next if l2 else None
    
    return dummy.next

# Time: O(max(m,n)), Space: O(max(m,n))
# Example: [2,4,3] + [5,6,4] = [7,0,8] (342 + 465 = 807)
```

## 4. Binary Tree Level Order Traversal (LeetCode #102)
**Problem:** Return level-order traversal of binary tree
```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def level_order(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result

# Time: O(n), Space: O(n)
# Example: [3,9,20,null,null,15,7] → [[3],[9,20],[15,7]]
```

## 5. Recover Binary Search Tree (LeetCode #99)
**Problem:** Fix BST where exactly two nodes are swapped
```python
def recover_tree(root):
    def inorder(node):
        if not node:
            return
        
        inorder(node.left)
        
        # Check if current violates BST property
        if self.prev and self.prev.val > node.val:
            if not self.first:
                self.first = self.prev
            self.second = node
        
        self.prev = node
        inorder(node.right)
    
    self.first = self.second = self.prev = None
    inorder(root)
    
    # Swap the values
    if self.first and self.second:
        self.first.val, self.second.val = self.second.val, self.first.val

# Time: O(n), Space: O(h) where h is height
# Morris traversal version can achieve O(1) space
```

## 6. Search in Rotated Sorted Array (LeetCode #33)
**Problem:** Search target in rotated sorted array
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1

# Time: O(log n), Space: O(1)
# Example: nums=[4,5,6,7,0,1,2], target=0 → 4
```

## 7. Palindrome Permutation (LeetCode #266)
**Problem:** Check if string can form a palindrome
```python
def can_permute_palindrome(s):
    from collections import Counter
    
    char_count = Counter(s)
    odd_count = sum(1 for count in char_count.values() if count % 2 == 1)
    
    return odd_count <= 1

# Alternative O(1) space solution
def can_permute_palindrome_v2(s):
    char_set = set()
    for char in s:
        if char in char_set:
            char_set.remove(char)
        else:
            char_set.add(char)
    
    return len(char_set) <= 1

# Time: O(n), Space: O(1) for ASCII chars
# Example: "aab" → True, "abc" → False
```

## 8. Copy List with Random Pointer (LeetCode #138)
**Problem:** Deep copy linked list with random pointers
```python
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random

def copy_random_list(head):
    if not head:
        return None
    
    # Step 1: Create interweaved list
    current = head
    while current:
        new_node = Node(current.val)
        new_node.next = current.next
        current.next = new_node
        current = new_node.next
    
    # Step 2: Set random pointers
    current = head
    while current:
        if current.random:
            current.next.random = current.random.next
        current = current.next.next
    
    # Step 3: Separate the lists
    dummy = Node(0)
    new_current = dummy
    current = head
    
    while current:
        new_current.next = current.next
        current.next = current.next.next
        new_current = new_current.next
        current = current.next
    
    return dummy.next

# Time: O(n), Space: O(1)
# Alternative HashMap solution: O(n) space but simpler logic
```

---

## Key Patterns & Tips

### **Array/Hash Patterns:**
- **Two Sum:** HashMap for O(n) complement lookup
- **Set Matrix Zeroes:** Use first row/col as markers for O(1) space

### **Linked List Patterns:**
- **Add Two Numbers:** Handle carry carefully, use dummy node
- **Copy Random List:** Interweaving technique for O(1) space

### **Tree Patterns:**
- **Level Order:** BFS with queue, track level size
- **BST Recovery:** Inorder traversal finds violations

### **Binary Search:**
- **Rotated Array:** Identify sorted half, narrow search space

### **String/Character Patterns:**
- **Palindrome Permutation:** At most one odd-count character

### **Interview Communication Tips:**
1. **Clarify inputs:** Ask about edge cases, constraints
2. **Explain approach:** Walk through algorithm before coding
3. **Code cleanly:** Use descriptive variable names
4. **Test examples:** Trace through with sample input
5. **Optimize:** Discuss time/space complexity improvements

---

