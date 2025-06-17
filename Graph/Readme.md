# Graph Interview Questions – Greg Hogg Playlist

---
## Visual Summary Table

| Problem                                 | LeetCode Link                                                                           | Companies                       |
|------------------------------------------|-----------------------------------------------------------------------------------------|----------------------------------|
| Find if Path Exists in Graph             | [LeetCode 1971](https://leetcode.com/problems/find-if-path-exists-in-graph/)            | Facebook, Microsoft, Google[3]   |
| Number of Islands                        | [LeetCode 200](https://leetcode.com/problems/number-of-islands/)                        | Amazon, Google, Facebook[4]     |
| Max Area of Island                       | [LeetCode 695](https://leetcode.com/problems/max-area-of-island/)                       | Google, Amazon[2]               |
| Course Schedule                          | [LeetCode 207](https://leetcode.com/problems/course-schedule/)                          | Facebook, Google, Amazon[5]     |
| Course Schedule II (Topological Sort)    | [LeetCode 210](https://leetcode.com/problems/course-schedule-ii/)                       | Google, Facebook, Microsoft[5]  |
| Pacific Atlantic Water Flow              | [LeetCode 417](https://leetcode.com/problems/pacific-atlantic-water-flow/)              | Google, Amazon[4]               |
| Clone Graph                              | [LeetCode 133](https://leetcode.com/problems/clone-graph/)                              | Amazon, Google, Facebook[6]     |
| Rotting Oranges                          | [LeetCode 994](https://leetcode.com/problems/rotting-oranges/)                          | Amazon, Microsoft[4]            |
| Min Cost to Connect All Points (Prim’s)  | [LeetCode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points/)          | Google, Amazon[4]               |
| Network Delay Time (Dijkstra’s)          | [LeetCode 743](https://leetcode.com/problems/network-delay-time/)                       | Google, Facebook[4]             |

---

## 1. Find if Path Exists in Graph

- **Description:** Given an undirected graph, determine if a valid path exists between two nodes using DFS or BFS[3].
- **LeetCode:** [LeetCode 1971](https://leetcode.com/problems/find-if-path-exists-in-graph/)
- **Companies:** Facebook, Microsoft, Google[3].

---

## 2. Number of Islands

- **Description:** Count the number of islands in a 2D grid using DFS/BFS to traverse connected land[4].
- **LeetCode:** [LeetCode 200](https://leetcode.com/problems/number-of-islands/)
- **Companies:** Amazon, Google, Facebook[4].

---

## 3. Max Area of Island

- **Description:** Find the largest area of a connected group of 1s (island) in a 2D grid using DFS/BFS[2].
- **LeetCode:** [LeetCode 695](https://leetcode.com/problems/max-area-of-island/)
- **Companies:** Google, Amazon[2].

---

## 4. Course Schedule

- **Description:** Determine if you can finish all courses given prerequisites (cycle detection in a directed graph)[5].
- **LeetCode:** [LeetCode 207](https://leetcode.com/problems/course-schedule/)
- **Companies:** Facebook, Google, Amazon[5].

---

## 5. Course Schedule II (Topological Sort)

- **Description:** Return the order to take courses given prerequisites using topological sort[5].
- **LeetCode:** [LeetCode 210](https://leetcode.com/problems/course-schedule-ii/)
- **Companies:** Google, Facebook, Microsoft[5].

---

## 6. Pacific Atlantic Water Flow

- **Description:** Find all grid coordinates where water can flow to both the Pacific and Atlantic oceans using DFS/BFS[4].
- **LeetCode:** [LeetCode 417](https://leetcode.com/problems/pacific-atlantic-water-flow/)
- **Companies:** Google, Amazon[4].

---

## 7. Clone Graph

- **Description:** Clone an undirected graph using DFS/BFS and a hashmap to track visited nodes[6].
- **LeetCode:** [LeetCode 133](https://leetcode.com/problems/clone-graph/)
- **Companies:** Amazon, Google, Facebook[6].

---

## 8. Rotting Oranges

- **Description:** Given a grid, return the minimum time to rot all oranges using BFS for level-order traversal[4].
- **LeetCode:** [LeetCode 994](https://leetcode.com/problems/rotting-oranges/)
- **Companies:** Amazon, Microsoft[4].

---

## 9. Min Cost to Connect All Points (Prim’s Algorithm)

- **Description:** Find the minimum cost to connect all points using Prim’s algorithm to construct a Minimum Spanning Tree (MST)[4].
- **LeetCode:** [LeetCode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points/)
- **Companies:** Google, Amazon[4].

---

## 10. Network Delay Time (Dijkstra’s Algorithm)

- **Description:** Find the time it takes for all nodes to receive a signal from a starting node using Dijkstra’s algorithm[4].
- **LeetCode:** [LeetCode 743](https://leetcode.com/problems/network-delay-time/)
- **Companies:** Google, Facebook[4].

---


---

## Graph Interview Tips

- **When to Use Graphs:** Use graphs for problems involving networks, connectivity, paths, and cycles[4].
- **Common Algorithms:** Master BFS, DFS, Dijkstra’s, Prim’s, and topological sort[4].
- **Practice:** Focus on problems involving traversal, cycle detection, shortest/longest paths, and connectivity[4].
- **Complexity:** BFS/DFS are generally O(V+E); Dijkstra’s and Prim’s are O(E log V) with heaps[4].

---

# Graph Interview Questions Solutions – Greg Hogg Playlist

This document provides a comprehensive and visually structured guide to common graph interview problems, including key observations, interview tips, and code snippets for each problem. The format is consistent with previous guides for seamless interview preparation.

---

## Visual Summary Table

| Problem                                 | LeetCode Link                                                                           | Companies                       |
|------------------------------------------|-----------------------------------------------------------------------------------------|----------------------------------|
| Find if Path Exists in Graph             | [LeetCode 1971](https://leetcode.com/problems/find-if-path-exists-in-graph/)            | Facebook, Microsoft, Google[1]   |
| Number of Islands                        | [LeetCode 200](https://leetcode.com/problems/number-of-islands/)                        | Amazon, Google, Facebook[2]     |
| Max Area of Island                       | [LeetCode 695](https://leetcode.com/problems/max-area-of-island/)                       | Google, Amazon[3]               |
| Course Schedule                          | [LeetCode 207](https://leetcode.com/problems/course-schedule/)                          | Facebook, Google, Amazon[4]     |
| Course Schedule II (Topological Sort)    | [LeetCode 210](https://leetcode.com/problems/course-schedule-ii/)                       | Google, Facebook, Microsoft[4]  |
| Pacific Atlantic Water Flow              | [LeetCode 417](https://leetcode.com/problems/pacific-atlantic-water-flow/)              | Google, Amazon[2]               |
| Clone Graph                              | [LeetCode 133](https://leetcode.com/problems/clone-graph/)                              | Amazon, Google, Facebook[5]     |
| Rotting Oranges                          | [LeetCode 994](https://leetcode.com/problems/rotting-oranges/)                          | Amazon, Microsoft[2]            |
| Min Cost to Connect All Points (Prim’s)  | [LeetCode 1584](https://leetcode.com/problems/min-cost-to-connect-all-points/)          | Google, Amazon[2]               |
| Network Delay Time (Dijkstra’s)          | [LeetCode 743](https://leetcode.com/problems/network-delay-time/)                       | Google, Facebook[2]             |

---

## 1. Find if Path Exists in Graph

**Key Observation:**  
This is a classic connectivity check in an undirected graph. The main insight is that you can use BFS, DFS, or Union-Find (Disjoint Set Union) to determine if two nodes are in the same connected component[1].

**Interview Tips:**  
- Be ready to discuss both BFS/DFS and Union-Find approaches.
- Interviewers may ask for time and space complexity (BFS/DFS: O(V+E), Union-Find: near O(1) per query with path compression).
- Expectation: clean code, correct visited set usage, and handling disconnected graphs[1].

**Code Snippet (BFS):**
```python
from collections import deque, defaultdict

def validPath(n, edges, source, destination):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    visited = set()
    queue = deque([source])
    while queue:
        node = queue.popleft()
        if node == destination:
            return True
        if node not in visited:
            visited.add(node)
            for neighbor in graph[node]:
                if neighbor not in visited:
                    queue.append(neighbor)
    return False
```


---

## 2. Number of Islands

**Key Observation:**  
Treat the 2D grid as a graph where each land cell ('1') is a node. Use DFS or BFS to traverse and mark all connected land cells as visited, counting each new traversal as a new island[6].

**Interview Tips:**  
- Be clear about marking visited cells (in-place or with a separate matrix).
- Interviewers expect you to handle grid boundaries and avoid double-counting.
- Discuss time complexity O(mn) and space complexity O(mn) for recursion stack or visited matrix[6].

**Code Snippet (DFS):**
```python
def numIslands(grid):
    if not grid:
        return 0
    m, n = len(grid), len(grid[0])
    def dfs(i, j):
        if i = m or j = n or grid[i][j] != '1':
            return
        grid[i][j] = '0'
        for x, y in [(0,1),(1,0),(-1,0),(0,-1)]:
            dfs(i+x, j+y)
    count = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    return count
```


---

## 3. Max Area of Island

**Key Observation:**  
The largest island is the largest connected group of 1s. Use DFS to explore each island, marking visited cells, and keep track of the largest area found[5][7].

**Interview Tips:**  
- Explain why DFS is efficient for this problem.
- Mention edge cases: all water, all land, or multiple small islands.
- Interviewers appreciate clear handling of visited state[5][7].

**Code Snippet (DFS):**
```python
def maxAreaOfIsland(grid):
    m, n = len(grid), len(grid[0])
    def dfs(i, j):
        if 0 <= i < m and 0 <= j < n and grid[i][j]:
            grid[i][j] = 0
            return 1 + sum(dfs(i+x, j+y) for x, y in [(0,1),(1,0),(-1,0),(0,-1)])
        return 0
    return max(dfs(i, j) for i in range(m) for j in range(n) if grid[i][j]) if grid else 0
```


---

## 4. Course Schedule

**Key Observation:**  
This is a cycle detection problem in a directed graph. If there's a cycle in prerequisites, it's impossible to finish all courses. Use DFS with a recursion stack or BFS with in-degree (Kahn’s algorithm)[8].

**Interview Tips:**  
- Be ready to explain both DFS and BFS (topological sort) approaches.
- Interviewers expect you to discuss cycle detection and graph representation.
- Time complexity: O(V+E)[8].

**Code Snippet (DFS):**
```python
def canFinish(numCourses, prerequisites):
    graph = [[] for _ in range(numCourses)]
    for a, b in prerequisites:
        graph[b].append(a)
    visited = [0] * numCourses
    def dfs(node):
        if visited[node] == 1:
            return False
        if visited[node] == 2:
            return True
        visited[node] = 1
        for nei in graph[node]:
            if not dfs(nei):
                return False
        visited[node] = 2
        return True
    return all(dfs(i) for i in range(numCourses) if visited[i] == 0)
```


---

## 5. Course Schedule II (Topological Sort)

**Key Observation:**  
This is a topological sort problem on a DAG. If a valid ordering exists (no cycles), return it. Use Kahn’s algorithm (BFS with in-degree)[8].

**Interview Tips:**  
- Interviewers expect you to know both BFS and DFS topological sort.
- Discuss cycle detection and what to return if a cycle exists.
- Time complexity: O(V+E)[8].

**Code Snippet (Kahn’s Algorithm):**
```python
from collections import deque, defaultdict

def findOrder(numCourses, prerequisites):
    graph = defaultdict(list)
    in_degree = [0] * numCourses
    for a, b in prerequisites:
        graph[b].append(a)
        in_degree[a] += 1
    queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
    res = []
    while queue:
        node = queue.popleft()
        res.append(node)
        for nei in graph[node]:
            in_degree[nei] -= 1
            if in_degree[nei] == 0:
                queue.append(nei)
    return res if len(res) == numCourses else []
```


---

## 6. Pacific Atlantic Water Flow

**Key Observation:**  
The trick is to reverse the flow: start BFS/DFS from the ocean borders and mark all cells that can reach each ocean. The answer is the intersection of cells reachable from both oceans[9].

**Interview Tips:**  
- Emphasize the dual traversal (one for each ocean).
- Interviewers expect discussion of visited sets and intersection logic.
- Time complexity: O(mn) for m x n grid[9].

**Code Snippet (DFS):**
```python
def pacificAtlantic(heights):
    if not heights: return []
    m, n = len(heights), len(heights[0])
    pac, atl = set(), set()
    def dfs(i, j, visited, prev):
        if (i, j) in visited or not (0 <= i < m and 0 <= j < n) or heights[i][j] < prev:
            return
        visited.add((i, j))
        for x, y in [(0,1),(1,0),(-1,0),(0,-1)]:
            dfs(i+x, j+y, visited, heights[i][j])
    for i in range(m):
        dfs(i, 0, pac, heights[i][0])
        dfs(i, n-1, atl, heights[i][n-1])
    for j in range(n):
        dfs(0, j, pac, heights[0][j])
        dfs(m-1, j, atl, heights[m-1][j])
    return list(pac & atl)
```


---

## 7. Clone Graph

**Key Observation:**  
To avoid infinite loops in graphs with cycles, use a hashmap to record already cloned nodes. Traverse with BFS or DFS, cloning each node and its neighbors[10][11].

**Interview Tips:**  
- Explain why a visited map is necessary.
- Interviewers expect a deep copy, not a shallow one.
- Discuss both BFS and DFS approaches[10].

**Code Snippet (DFS):**
```python
def cloneGraph(node):
    if not node:
        return None
    visited = {}
    def dfs(n):
        if n in visited:
            return visited[n]
        copy = Node(n.val)
        visited[n] = copy
        for nei in n.neighbors:
            copy.neighbors.append(dfs(nei))
        return copy
    return dfs(node)
```


---

## 8. Rotting Oranges

**Key Observation:**  
Model the grid as a graph and use BFS to spread rot level by level (minute by minute). All initially rotten oranges are BFS sources[12].

**Interview Tips:**  
- Discuss why BFS is optimal for shortest time propagation.
- Interviewers expect you to handle grid boundaries and unreachable oranges.
- Time complexity: O(mn)[12].

**Code Snippet (BFS):**
```python
from collections import deque

def orangesRotting(grid):
    m, n = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 2:
                queue.append((i, j))
            elif grid[i][j] == 1:
                fresh += 1
    minutes = 0
    while queue and fresh:
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
                nx, ny = x+dx, y+dy
                if 0<=nx<m and 0<=ny<n and grid[nx][ny]==1:
                    grid[nx][ny]=2
                    fresh -= 1
                    queue.append((nx, ny))
        minutes += 1
    return minutes if fresh == 0 else -1
```


---

## 9. Min Cost to Connect All Points (Prim’s Algorithm)

**Key Observation:**  
Model the points as a fully connected graph with Manhattan distances as edge weights. Use Prim’s algorithm to build a Minimum Spanning Tree (MST) for minimum total cost[13].

**Interview Tips:**  
- Be ready to explain Prim’s vs. Kruskal’s MST algorithms.
- Interviewers may ask for heap usage and complexity: O(N^2) for dense graphs[13].

**Code Snippet (Prim's Algorithm):**
```python
import heapq

def minCostConnectPoints(points):
    n = len(points)
    visited = set()
    minHeap = [(0, 0)]  # (cost, point)
    res = 0
    while len(visited) < n:
        cost, i = heapq.heappop(minHeap)
        if i in visited:
            continue
        res += cost
        visited.add(i)
        for j in range(n):
            if j not in visited:
                dist = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
                heapq.heappush(minHeap, (dist, j))
    return res
```


---

## 10. Network Delay Time (Dijkstra’s Algorithm)

**Key Observation:**  
Use Dijkstra’s algorithm to find the shortest time from the source to all nodes in a weighted directed graph. The answer is the maximum shortest time to any node[2].

**Interview Tips:**  
- Explain the use of a min-heap (priority queue) for efficient Dijkstra’s.
- Interviewers expect you to handle unreachable nodes (return -1 if any node is not reached).
- Time complexity: O(E log V)[2].

**Code Snippet (Dijkstra’s Algorithm):**
```python
import heapq
from collections import defaultdict

def networkDelayTime(times, n, k):
    graph = defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))
    heap = [(0, k)]
    dist = {}
    while heap:
        time, node = heapq.heappop(heap)
        if node in dist:
            continue
        dist[node] = time
        for nei, wt in graph[node]:
            if nei not in dist:
                heapq.heappush(heap, (time + wt, nei))
    return max(dist.values()) if len(dist) == n else -1
```


---

## Graph Interview Tips

- **When to Use Graphs:** Use graphs for problems involving networks, connectivity, paths, and cycles[2].
- **Common Algorithms:** Master BFS, DFS, Dijkstra’s, Prim’s, and topological sort[2].
- **Practice:** Focus on problems involving traversal, cycle detection, shortest/longest paths, and connectivity[2].
- **Complexity:** BFS/DFS are generally O(V+E); Dijkstra’s and Prim’s are O(E log V) with heaps[2].

---



---
