---

## 🚀 Interview Prep Cheat Sheet: Key Ideas & Algorithms

Here are the core concepts and data structures needed to tackle these common interview problems.

### ## 🔑 Hash Maps & Sets

1.  **Two Sum (Easy)**
    * **Idea:** Use a hash map to store `(value, index)` as you iterate. For each element `x`, check if `target - x` is already in the map.
    * **Complexity:** $O(n)$ Time, $O(n)$ Space.

2.  **Longest Substring Without Repeating Characters (Medium) 🪟**
    * **Idea:** Use a **sliding window** with a hash set (or map) to keep track of characters in the current window. Expand the window with a right pointer; shrink from the left if a character is repeated.
    * **Complexity:** $O(n)$ Time, $O(min(n, m))$ Space (m = charset size).

3.  **Longest Consecutive Sequence (Medium)**
    * **Idea:** Add all numbers to a hash set for $O(1)$ lookups. Iterate through the numbers; if a number `x` is the start of a sequence (i.e., `x-1` is not in the set), start counting upwards (`x+1`, `x+2`, ...) to find the sequence length.
    * **Complexity:** $O(n)$ Time, $O(n)$ Space.

---

### ## 📚 Stacks, Queues & Heaps

4.  **LRU Cache (Medium) 🔑**
    * **Idea:** Combine a **hash map** and a **doubly linked list**. The map provides $O(1)$ access to nodes. The list maintains the order of use; move nodes to the front on access, remove from the tail when capacity is reached.
    * **Complexity:** $O(1)$ for get and put operations.

5.  **Valid Parentheses (Easy)**
    * **Idea:** Use a **stack**. Push opening brackets `(`, `{`, `[` onto the stack. When a closing bracket is encountered, pop from the stack and check if it's the matching pair.
    * **Complexity:** $O(n)$ Time, $O(n)$ Space.

6.  **Rotting Oranges (Medium) 🗺️**
    * **Idea:** Use **Breadth-First Search (BFS)**. Start a multi-source BFS from all initially rotten oranges (`2`s). Each "level" of the BFS represents one minute passing. Keep a count of fresh oranges.
    * **Complexity:** $O(m \times n)$ Time, $O(m \times n)$ Space.

7.  **Top K Frequent Elements (Medium) 🔑**
    * **Idea:** Use a **hash map** to count frequencies. Then, use a **min-heap** of size `k` to keep track of the top `k` elements, or use **bucket sort** where the array index represents frequency.
    * **Complexity (Heap):** $O(n \log k)$ Time, $O(n)$ Space.

---

### ## 🔍 Searching & Sorting

8.  **Search in Rotated Sorted Array (Medium)**
    * **Idea:** Use a modified **binary search**. In each step, determine which half of the array (`mid` to `right` or `left` to `mid`) is sorted. Check if the target lies in the sorted half to decide where to search next.
    * **Complexity:** $O(\log n)$ Time, $O(1)$ Space.

9.  **Merge Intervals (Medium)**
    * **Idea:** **Sort** the intervals based on their start times. Iterate through the sorted intervals and merge the current interval with the previous one if they overlap.
    * **Complexity:** $O(n \log n)$ Time, $O(n)$ Space.

10. **Median of Two Sorted Arrays (Hard)**
    * **Idea:** Use **binary search** on the smaller array to find the optimal partition point. The goal is to divide both arrays into two halves such that all elements in the combined left halves are less than or equal to all elements in the combined right halves.
    * **Complexity:** $O(\log(min(m, n)))$ Time, $O(1)$ Space.

11. **Sort Colors (Medium) 🔄**
    * **Idea:** The **Dutch National Flag algorithm**. Use three pointers (`low`, `mid`, `high`). Iterate with `mid`: swap `0`s to the `low` end, `2`s to the `high` end, and leave `1`s in place.
    * **Complexity:** $O(n)$ Time, $O(1)$ Space.

---

### ## 💡 DP & Recursion

12. **Generate Parentheses (Medium)**
    * **Idea:** Use **backtracking**. Build the string recursively, keeping track of the counts of open and closed parentheses. Add `(` if `open < n`. Add `)` if `close < open`.
    * **Complexity:** $O(\frac{4^n}{\sqrt{n}})$ Time (Catalan numbers), $O(n)$ Space for recursion depth.

13. **Word Break (Medium)**
    * **Idea:** Use **Dynamic Programming**. Create a `dp` boolean array where `dp[i]` is true if the substring `s[0...i-1]` can be broken down. The state transition is `dp[i] = dp[j] && dictionary.contains(s[j...i-1])` for `0 <= j < i`.
    * **Complexity:** $O(n^2)$ Time, $O(n)$ Space.

14. **Decode Ways (Medium)**
    * **Idea:** Use **Dynamic Programming**. `dp[i]` is the number of ways to decode `s[0...i-1]`. The transition depends on the last one or two characters: `dp[i] = dp[i-1]` (if one-digit is valid) `+ dp[i-2]` (if two-digit is valid).
    * **Complexity:** $O(n)$ Time, $O(n)$ Space (can be optimized to $O(1)$ space).

15. **Longest Common Subsequence (Medium)**
    * **Idea:** Use 2D **Dynamic Programming**. `dp[i][j]` stores the length of the LCS for `text1[0...i-1]` and `text2[0...j-1]`. If chars match, `1 + dp[i-1][j-1]`. If not, `max(dp[i-1][j], dp[i][j-1])`.
    * **Complexity:** $O(m \times n)$ Time, $O(m \times n)$ Space.

---

### ## 🌳 Trees

16. **Diameter of Binary Tree (Easy)**
    * **Idea:** Use **Depth-First Search (DFS)**. For each node, the path through it is `height(left) + height(right)`. The diameter is the maximum of these path lengths found across all nodes. A post-order traversal is natural for this.
    * **Complexity:** $O(n)$ Time, $O(h)$ Space (h = tree height).

17. **Construct Binary Tree from Inorder and Postorder Traversal (Medium)**
    * **Idea:** Use **recursion**. The last element of the postorder array is the root. Find this root in the inorder array; elements to its left form the left subtree, and elements to its right form the right subtree. Recurse on the corresponding subarrays.
    * **Complexity:** $O(n)$ Time, $O(n)$ Space.

18. **Populating Next Right Pointers in Each Node (Medium)**
    * **Idea (Perfect Tree):** Use a **DFS** approach. For a node, set `node.left.next = node.right`. If `node.next` exists, set `node.right.next = node.next.left`.
    * **Idea (Any Tree):** Use level order traversal (**BFS**) with a queue. For each level, connect nodes sequentially.
    * **Complexity:** $O(n)$ Time, $O(1)$ Space (for perfect tree), $O(w)$ Space for BFS (w=max width).

---

### ## 🗺️ Matrix & Grids

19. **Set Matrix Zeroes (Medium)**
    * **Idea:** Use the first row and first column as markers to avoid using $O(m+n)$ extra space. Use two boolean flags to track if the first row/column themselves need to be zeroed.
    * **Complexity:** $O(m \times n)$ Time, $O(1)$ Space.

20. **Number of Islands (Medium)**
    * **Idea:** Iterate through the grid. When you find land ('1'), increment an island counter and start a **DFS or BFS** to find all connected parts of that island, marking them as visited (e.g., changing '1' to '0') to avoid recounting.
    * **Complexity:** $O(m \times n)$ Time, $O(m \times n)$ Space (recursion stack).

21. **Spiral Matrix (Medium)**
    * **Idea:** Use four pointers to represent the boundaries of the matrix (`top`, `bottom`, `left`, `right`). Traverse in a spiral layer by layer (right -> down -> left -> up), shrinking the boundaries after each direction is completed.
    * **Complexity:** $O(m \times n)$ Time, $O(1)$ Space.

22. **Diagonal Traverse (Medium)**
    * **Idea:** Simulate the traversal. Notice that elements on the same diagonal have the same `row + col` sum. Diagonals with even sums are traversed upwards, and diagonals with odd sums are traversed downwards. Handle boundary conditions carefully.
    * **Complexity:** $O(m \times n)$ Time, $O(1)$ Space.

23. **Sort the Matrix Diagonally (Medium)**
    * **Idea:** Group elements by their diagonal ID (`row - col`). A **hash map** where keys are diagonal IDs and values are lists (or min-heaps) of the elements is perfect. Sort each list and place the elements back into the matrix.
    * **Complexity:** $O(m \times n \log(\min(m,n)))$ Time, $O(m \times n)$ Space.

---

### ## 🔄 Array & String Manipulation

24. **Trapping Rain Water (Hard)**
    * **Idea:** Use the **two-pointer** technique. Maintain `left_max` and `right_max` heights from both ends. The water trapped at any point is determined by the shorter of the two maxes.
    * **Complexity:** $O(n)$ Time, $O(1)$ Space.

25. **Minimum Window Substring (Hard) 🪟**
    * **Idea:** A **sliding window** with two pointers and a hash map. Expand the window with the right pointer until it's valid (contains all characters from the target string). Then, shrink the window from the left to find the smallest possible valid window.
    * **Complexity:** $O(S + T)$ Time, $O(S + T)$ Space.

26. **Integer to Roman (Medium)**
    * **Idea:** Create a mapping of integer values to Roman symbols (e.g., 1000 to "M", 900 to "CM"). Iterate through the values from largest to smallest, greedily subtracting them from the input number and appending the corresponding symbol.
    * **Complexity:** $O(1)$ Time and Space (since the number of symbols is fixed).

27. **Next Greater Element III (Medium)**
    * **Idea:** Same logic as the "Next Permutation" algorithm.
        1. Find the first digit from the right that is smaller than the digit to its right (the pivot).
        2. Find the smallest digit to the right of the pivot that is larger than the pivot.
        3. Swap them.
        4. Reverse the sequence of digits to the right of the original pivot position.
    * **Complexity:** $O(k)$ Time, $O(k)$ Space (where k is the number of digits).

28. **Zigzag Conversion (Medium)**
    * **Idea:** Create an array of `numRows` string builders. Iterate through the input string, appending each character to the appropriate string builder. Change direction (down/up) when the row index hits `0` or `numRows - 1`.
    * **Complexity:** $O(n)$ Time, $O(n)$ Space.
