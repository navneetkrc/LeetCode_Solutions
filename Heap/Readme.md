# Heap Interview Questions – Greg Hogg Playlist

This document summarizes key heap-related interview questions from Greg Hogg’s YouTube playlist on Heaps, providing concise descriptions, LeetCode links, and companies where each problem is frequently asked. Use this as a visually rich and practical guide for interview preparation.

---

## Table of Contents

1. [Top K Frequent Elements](#top-k-frequent-elements)
2. [K Closest Points to Origin](#k-closest-points-to-origin)
3. [Merge K Sorted Linked Lists](#merge-k-sorted-linked-lists)
4. [Sort Characters By Frequency](#sort-characters-by-frequency)
5. [Kth Largest Element in an Array](#kth-largest-element-in-an-array)
6. [Meeting Rooms II](#meeting-rooms-ii)
7. [Build a Max Heap from Array](#build-a-max-heap-from-array)

---

## 1. Top K Frequent Elements

- **Description:** Find the k most frequent elements in an integer array using a heap for efficient frequency tracking.
- **LeetCode:** [LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)
- **Companies:** Amazon, Google, Facebook[1][2]

---

## 2. K Closest Points to Origin

- **Description:** Given a list of points on a plane, return the k closest points to the origin using a min-heap to efficiently track distances.
- **LeetCode:** [LeetCode 973](https://leetcode.com/problems/k-closest-points-to-origin/)
- **Companies:** Facebook, Google, Microsoft[2][3]

---

## 3. Merge K Sorted Linked Lists

- **Description:** Merge k sorted linked lists into one sorted list using a min-heap to always select the smallest current node.
- **LeetCode:** [LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/)
- **Companies:** Google, Amazon, Microsoft[4][2]

---

## 4. Sort Characters By Frequency

- **Description:** Rearrange characters in a string so that characters appear in decreasing order of frequency using a max-heap.
- **LeetCode:** [LeetCode 451](https://leetcode.com/problems/sort-characters-by-frequency/)
- **Companies:** Google, Amazon, Oracle, Zoho[5][2]

---

## 5. Kth Largest Element in an Array

- **Description:** Find the kth largest element in an unsorted array using a min-heap of size k for optimal efficiency.
- **LeetCode:** [LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/)
- **Companies:** Amazon, Facebook, Microsoft[2]

---

## 6. Meeting Rooms II

- **Description:** Given meeting time intervals, determine the minimum number of conference rooms required using a heap to track end times.
- **LeetCode:** [LeetCode 253](https://leetcode.com/problems/meeting-rooms-ii/)
- **Companies:** Google, Facebook, Bloomberg[2]

---

## 7. Build a Max Heap from Array

- **Description:** Transform an array into a max-heap in-place, a foundational heap operation often required in interviews.
- **LeetCode:** [LeetCode 1046 (related: Last Stone Weight)](https://leetcode.com/problems/last-stone-weight/)
- **Companies:** Amazon, Google[2]

---

## Heap Interview Tips

- **When to Use Heaps:** Use heaps for problems involving running minimum/maximum, priority queues, and extracting k largest/smallest elements efficiently[2].
- **Common Pitfalls:** Choose the correct heap type (min/max), maintain heap property after operations, and remember heap time complexities[2].
- **Heap Operations:** Insertion, deletion, and heapify are all O(log n); building a heap is O(n)[2].

---

## Visual Summary Table

| Problem                          | LeetCode Link                                                                    | Companies                         |
|-----------------------------------|----------------------------------------------------------------------------------|-----------------------------------|
| Top K Frequent Elements           | [LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)           | Amazon, Google, Facebook[1][2]    |
| K Closest Points to Origin        | [LeetCode 973](https://leetcode.com/problems/k-closest-points-to-origin/)        | Facebook, Google, Microsoft[2][3] |
| Merge K Sorted Linked Lists       | [LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/)               | Google, Amazon, Microsoft[4][2]   |
| Sort Characters By Frequency      | [LeetCode 451](https://leetcode.com/problems/sort-characters-by-frequency/)       | Google, Amazon, Oracle[5][2]     |
| Kth Largest Element in an Array   | [LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/)    | Amazon, Facebook, Microsoft[2]    |
| Meeting Rooms II                  | [LeetCode 253](https://leetcode.com/problems/meeting-rooms-ii/)                   | Google, Facebook, Bloomberg[2]    |
| Build a Max Heap from Array       | [LeetCode 1046](https://leetcode.com/problems/last-stone-weight/)                 | Amazon, Google[2]                 |

---

## Additional Resources

- [Heap Interview Questions & Tips](https://interviewing.io/heaps-interview-questions)[2]
- [Heap (Priority Queue) | LeetCode The Hard Way](http://leetcodethehardway.com/tutorials/basic-topics/heap)[6]

---
